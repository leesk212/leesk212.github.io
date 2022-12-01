---
tags: cloud serverless architecture
toc: True
---

# What is Serverelss?
# What is Firecracker?
## Paper Review: "Firecracker: Lightweight Virtualization for Serverless Applications" 

> ref: https://www.micahlerner.com/2021/06/17/firecracker-lightweight-virtualization-for-serverless-applications.html

* Firecracker is a high-performance virtualization solution built to run Amazon’s serverless** applications securely and with minimal resources. It now does so at immense scale (at the time the paper was published, it supported “millions of production workloads, and trillions of requests per month”). { ** Serverless meaning that the resources for running a workload are provided on-demand, rather than being paid for over a prolonged time-period. Martin Fowler has some great docs on the topic [here](https://martinfowler.com/articles/serverless.html).}
> Firecracker는 Amazon의 serverless 어플리케이션들을 보안적으로 그리고 적은 자원들로 실행하기 위해서 건설한 "고성능의 가상화 솔루션"이다. 이것은 현재 엄청난 크기의 규모로 실행하고 있습니다. (** 서버리스는 워크로드를 실행하기 위한 리소스를 장기간에 걸쳐 비용을 지불하는 대신, on-demand 방식으로 필요할때 자원을 제공한다.)

### What are the paper's contributions?
Contribution: 
* the Firecracker system itself
* the usage of Firecracker to power AWS Lambda

Motivation:
* Originally, Lambda functions ran on a separate virtual machine (VM) for every customer (although functions from the same customer would run in the same VM). Allocating a separate VM for every customer was great for isolating customers from each other - you wouldn’t want Company A to access Company B’s code or functionality, nor for Company A’s greedy resource consumption to starve Company B’s Lambdas of resources.
> 기존에, Lambda 함수는 모든 customer들에 대하여 별도의 가상 머신 (VM)에서 실행되었다. 동일한 customer의 Lambda 함수는 모두 동일한 VM에서 실행되었다. 그렇기에 모든 고객에 대해서 별도의 VM을 할당하는 것은 고객을 서로 격리하는데 유용했다. - 예를 들어, A회사의 Lambda function이 매우 많은 양의 resource를 사용해도 회사 B의 Lambda resource가 고갈되지는 않을 것이다.

![image](https://user-images.githubusercontent.com/67637935/204976020-b02f2fdd-af86-4126-8f84-2a04eac0bc82.png)

* Unfortunately, existing VM solutions required significant resources, and resulted in non-optimal utilization. For example, a customer might have a VM allocated to them, but the VM is not frequently used. Even though the VM isn’t used to its full capacity, there is still memory and CPU being consumed to run the VM. The Lambda system in this form was less-efficient, meaning it required more resources to scale (likely making the system more expensive for customers).
> 하지만, 불행하게도 이런 형태의 VM solution들은 상당한 리소스를 필요로 했고, 활용도가 최적화되지 않았다. 예를들어 고객에게 할당된 VM이 있을 수 있지만 해당 VM은 자주 사용되지 않았고, VM이 전체 용량을 실행하지 않더라도 VM을 실행하는데 여전히 메모리와 CPU가 사용된다. 이 형태 때문에 Lambda 시스템의 효율성이 떨어졌고, 확장하는데 더 많은 리소스가 필요하게 되었다. 

So, Goal: increasing utilization (and lowering cost), the team established constraints of possible future solution:

1. Overhead and density: Run “thousands of functions on a single machine, with minimal waste”. In other words, solving one of the main problems of the existing architecture.
> 1. 오버헤드 및 밀도: "최소한의 낭비로 단일 시스템에서 수천 개의 기능"을 실행합니다. 즉, 기존 아키텍처의 주요 문제 중 하나를 해결합니다.

2. Isolation: Ensure that applications are completely separate from one another (can’t read each other’s data, nor learn about them through side channels). The existing solution had this property, but at high cost.
> 2. 격리: 애플리케이션이 서로 완전히 분리되어 있는지 확인합니다(서로의 데이터를 읽을 수 없으며 사이드 채널을 통해 이에 대해 알 수 없음). 기존 솔루션은 이러한 속성을 가지고 있었지만 비용이 많이 들었습니다.

3. Performance: A new solution should have the same or better performance as before.
> 3. 성능: 새 솔루션은 이전과 동일하거나 더 나은 성능을 제공해야 합니다.

4. Compatibility: Run any binary “without code changes or recompilation”. 
> 4. 호환성: "코드 변경이나 재컴파일 없이" 모든 바이너리를 실행합니다.

5. Fast Switching: “It must be possible to start new functions and clean up old functions quickly”.
> 5. 빠른 전환: "새로운 기능을 시작하고 이전 기능을 신속하게 정리할 수 있어야 합니다."

6. Soft Allocation: “It must be possible to over commit CPU, memory, and other resources”. This requirement impacts utilization (and in turn, the cost of the system to AWS/the customer). Overcommittment comes into play a few times during a Firecracker VM’s lifetime. For example, when it starts up, it theoretically is allocated resources, but may not be using them right away if it is performing set up work. Other times, the VM may need to burst above the configured soft-limit on resources, and would need to consume those of another VM. The paper note’s “We have tested memory and CPU oversubscription ratios of over 20x, and run in production with ratios as high as 10x, with no issues” - very neat!
> 6. 소프트 할당: "CPU, 메모리 및 기타 리소스를 초과 할당할 수 있어야 합니다." 이 요구 사항은 활용도에 영향을 미치며 결과적으로 AWS/고객에 대한 시스템 비용에도 영향을 미칩니다. Firecracker VM의 수명 동안 오버 커밋이 몇 번 발생합니다. 예를 들어 시작할 때 이론적으로 리소스가 할당되지만 설정 작업을 수행하는 경우 바로 사용하지 않을 수 있습니다. __다른 경우에는 VM이 리소스에 대해 구성된 소프트 제한을 초과하여 버스트해야 할 수 있으며 다른 VM의 리소스를 소비__ 해야 합니다. 논문은 "우리는 20배 이상의 메모리 및 CPU 초과 사용률을 테스트했으며 문제 없이 최대 10배의 비율로 프로덕션 환경에서 실행했습니다." - 매우 깔끔합니다!

<span style="color:blue"> 기존 VM에서 이런 soft allocation은 못잡을듯 한데..? 얘를들어 cryptojacking이 guestos에 설치가 되어서 자원을 떙겨썼어 그럼 기존 guest os에 설치되어서 탐지하는 것은 잘 안될 것 같은데.. 그리고 갑자기 든 생각인데 systemcall로 잡는 기법들은 해당 microVM 자체에서 systemcall을 제한하는 기법들 때문에 안될 듯? --> 그니까 cpu execution port를 활용한다 정도? 아무튼 guestOS에 설치 되어서 탐지하는 것의 serverless 특징을 조금 더 생각해보자 </span>
