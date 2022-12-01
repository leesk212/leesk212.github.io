---
tags: cloud serverless architecture
toc: True
---

# What is Serverelss?

> ref: * Li, Zijun, et al. "The serverless computing survey: A technical primer for design architecture." ACM Computing Surveys (CSUR) 54.10s (2022).

![image](https://user-images.githubusercontent.com/67637935/204979420-57823419-14a7-479f-814b-2a7fa3b972e3.png)

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

* **The constraints were applied to three different categories of solutions: Linux containers, language-specific isolation, and alternative virtualization solutions (they were already using virtualization, but wanted to consider a different option than their existing implementation).**
> **제약 조건은 1. Linux Container, 2. 언어별 격리 및 3. 대체 가상화 솔루션(이미 가상화를 사용하고 있었지만 기존 구현과 다른 옵션을 고려하기를 원함)의 세 가지 솔루션 범주에 적용되었습니다.**

#### 1. Linux Containter
* There are several Isolation downsides to using Linux containers.
> Linux Container를 사용하면 Isolation에 몇가지 단점이 있다. 

* First, Linux containers interact directly with a host OS using syscalls.
> 첫번째로 Linux container는 hostOS의 syscall을 사용하여 직접적으로 상호작용한다. 

* One can lock-down which syscalls a program can make (the paper mentions using Seccomp BPF), and even which arguments the syscalls can use, as well as using other security features of container systems (the Fly.io article linked above discusses this topic in more depth).
> 특정 사람은 프로그램이 만들 수 있는 syscalls 들을 잠글 수 있다. (논문에서는 Seccomp BPF 사용을 언급함), 그리고 심지어 syscalls가 사용할 수 있는 인수는 물론 컨테이너 시스템의 다른 보안 기능(위에 링크된 Fly.io 기사에서 이 주제에 대해 설명함)을 또한 잠글 수 있다.

* Even using other Linux isolation features, at the end of the day the container is still interacting with the OS. That means that if customer code in the container figures out a way to pwn the OS, or figures out a side channel to determine state of another container, Isolation might break down. Not great.
> 다른 Linux 격리 기능을 사용하더라도 결국 컨테이너는 여전히 OS와 상호 작용합니다. 즉, 컨테이너의 고객 코드가 OS를 소유하는 방법을 알아내거나 다른 컨테이너의 상태를 결정하기 위해 사이드 채널을 알아내면 격리가 깨질 수 있습니다. 좋지않다. 

#### 2. Language-specific isolation
* While there are ways to run language-specific VMs (like the JVM for Java/Scala/Clojure or V8 for Javascript), this approach doesn’t scale well to many different languages (nor does it allow for a system that can run arbitrary binaries - one of the original design goals).
> 언어별 VM(Java/Scala/Clojure용 JVM 또는 Javascript용 V8)을 실행하는 방법이 있지만 이 접근 방식은 다양한 언어로 잘 확장되지 않으며 임의의 바이너리를 실행할 수 있는 시스템도 허용하지 않습니다. - 원래 디자인 목표 중 하나).

#### 3. Alternative Virtualization Solutions <span style="color:blue">("traditional VM과의 변경점으로 시사할 수 있을 것 같음")</span>
* Revisiting virtualization led to a focus on what about the existing virtualization approach was holding Lambda back:
> 가상화를 재검토하면서 기존 가상화 접근 방식이 Lambda를 방해하는 요소에 초점을 맞췄습니다.
  * Isolation:  the code associated with the components of virtualization are lengthy (meaning more possible areas of exploitation), and [researchers have escaped from virtual machines before](https://www.computerworld.com/article/3182877/pwn2own-ends-with-two-virtual-machine-escapes.html).
  * > 격리: 가상화 구성 요소와 관련된 코드가 길고(악용 가능한 영역이 더 많다는 의미) 연구자들은 이전에 가상 머신에서 탈출했습니다.
  * Overhead and density: the components of virtualization (which we will get into further down) require too many resources, leading to low utilization
  * > 오버헤드 및 밀도: 가상화 구성 요소(아래에서 자세히 설명)에는 너무 많은 리소스가 필요하므로 활용도가 낮습니다.
  * Fast switching: VMs take a while to boot and shut down, which doesn’t mesh well with Lambda functions that need a VM quickly and may only use it for a few seconds (or less).
  * > 빠른 전환: VM을 부팅하고 종료하는 데 시간이 걸리므로 VM이 빨리 필요한 Lambda 기능과 잘 맞지 않고 몇 초(또는 그 미만) 동안만 사용할 수 있습니다.

* The team then applied the above requirements to the main components of the virtualization system: the hypervisor and the virtual machine monitor.
> 그런 다음 팀은 위의 요구 사항을 가상화 시스템의 주요 구성 요소인 하이퍼바이저 및 가상 머신 모니터에 적용했습니다.

* First, the team considered which type of hypervisor to choose. There are two types of hypervisors, Type 1 and Type 2. The textbook definitions of hypervisors say that Type 1 hypervisors are integrated directly in the hardware, while Type 2 hypervisors run an operating system on top of the hardware (then run the hypervisor on top of that operating system).
> 먼저 팀은 선택할 하이퍼바이저 유형을 고려했습니다. 유형 1과 유형 2의 두 가지 유형의 하이퍼바이저가 있습니다. 하이퍼바이저에 대한 교과서 정의에서는 유형 1 하이퍼바이저가 하드웨어에 직접 통합되는 반면 유형 2 하이퍼바이저는 하드웨어 위에서 운영 체제를 실행(그런 다음 하이퍼바이저를 맨 위에서 실행)한다고 말합니다. 해당 운영 체제).

![image](https://user-images.githubusercontent.com/67637935/204983883-f76de1ab-72cb-4f96-9db5-681421fe149d.png)

* Linux has a robust hypervisor built into the kernel, called Kernel Virtual Machine (a.k.a. KVM) that is arguably a Type 1 hypervisor
> Linux에는 유형 1 하이퍼바이저인 커널 가상 머신(일명 KVM)이라는 커널에 내장된 강력한 하이퍼바이저가 있습니다.

* Using a hypervisor like KVM allows for kernel components to be moved into userspace - if the kernel components are in user space and they get pwned, the host OS itself hasn’t been pwned, Linux provides an interface, virtio, that allows the user space kernel components to interact with the host OS.
> KVM과 같은 하이퍼바이저를 사용하면 커널 구성 요소를 사용자 공간으로 이동할 수 있습니다. - 커널 구성 요소가 사용자 공간에 있고 pwned되면 호스트 OS 자체는 pwned(완패)되지 않습니다. Linux는 사용자 공간 커널 구성 요소가 호스트 OS와 상호 작용할 수 있도록 하는 인터페이스인 virtio를 제공합니다

* Rather than passing all interactions with a guest kernel directly to the host kernel, some functions, in particular device interactions, go from a guest kernel to a virtual machine monitor (a.k.a. VMM). One of the most popular VMMs is QEMU.
> 게스트 커널과의 모든 상호 작용을 호스트 커널로 직접 전달하는 대신 일부 기능, 특히 장치 상호 작용은 게스트 커널에서 가상 머신 모니터(일명 VMM)로 이동합니다. 가장 인기 있는 VMM 중 하나는 QEMU입니다.

![image](https://user-images.githubusercontent.com/67637935/204984488-6e4fba31-bb91-4231-983b-249078895bb8.png)

* Unfortunately, QEMU has a significant amount of code (again, more code means more potential attack surface), as it supports a full range of functionality - even functionality that a Lambda would never use, like USB drivers.
> 불행하게도 QEMU는 USB 드라이버와 같이 Lambda가 절대 사용하지 않는 기능까지 모든 범위의 기능을 지원하기 때문에 상당한 양의 코드(코드가 많을수록 잠재적인 공격 표면이 더 커짐)를 가지고 있습니다.

* Rather than trying to pare down QEMU, the team forked crosvm (a VMM open-sourced by Google, and developed for ChromeOS),  in the process significantly rewriting core functionality for Firecracker’s use case.
> 팀은 QEMU를 줄이는 대신 crosvm(Google이 오픈 소스로 제공하고 ChromeOS용으로 개발한 VMM)을 포크하여 Firecracker 사용 사례의 핵심 기능을 대폭 재작성했습니다.

* The end result was a slimmer library with only code that would conceivably be used by a Lambda - resulting in 50k lines of Rust (versus > 1.4 million lines of C in QEMU)
> 최종 결과는 Lambda에서 사용할 수 있는 코드만 있는 더 슬림한 라이브러리였습니다. 결과적으로 50k 줄의 Rust가 생성되었습니다(QEMU에서 > 140만 줄의 C와 비교).

* Because the goal of Firecracker is to be as small as possible, the paper calls the project a MicroVM, rather than “VM”.
> Firecracker의 목표는 가능한 한 작게 만드는 것이기 때문에 논문에서는 이 프로젝트를 "VM"이 아닌 MicroVM이라고 부릅니다.

## How do Firecracker MicroVMs get run on AWS?
* Now that we roughly understand how Firecracker works, let’s dive into how it is used in running Lambda. First, we will look at how the Lambda architecture works on a high level, followed by a look at how the running the Lambda itself works.
> 이제 Firecracker의 작동 방식을 대략적으로 이해했으므로 Lambda를 실행하는 데 Firecracker가 어떻게 사용되는지 살펴보겠습니다. 먼저 Lambda 아키텍처가 높은 수준에서 작동하는 방식을 살펴본 다음 Lambda 실행 자체가 작동하는 방식을 살펴봅니다.

### High-level architecture of AWS Lambda
* When a developer runs (or Invokes, in AWS terminology) a Lambda, the ensuing HTTP request hits an AWS Load Balancer** ( ** Lambda는 '스토리지(S3), 대기열(SQS), 스트리밍 데이터(Kinesis) 및 데이터베이스(DynamoDB) 서비스를 포함한 다른 AWS 서비스와의 통합'과 같은 다른 이벤트를 통해 시작할 수도 있습니다.)
> 개발자가 Lambda를 실행(또는 AWS 용어로 호출)하면 후속 HTTP 요청이 AWS Load Balancer에 도달합니다.

![image](https://user-images.githubusercontent.com/67637935/204987064-7ca59d08-6f97-4fde-af5f-8824dea5bc05.png)

* There are a four main infrastructure components involved in running a Lambda once it has been invoked:
> 호출된 후 Lambda 실행과 관련된 네 가지 주요 인프라 구성 요소가 있습니다.

  1.  Workers: The components that actually run a Lambda’s code. Each worker runs many MicroVMs in “slots”, and other services schedule code to be run in the MicroVMs when a customer Invokes a Lambda.
  * > Workers: 실제로 Lambda의 코드를 실행하는 구성 요소입니다. 각 작업자는 "슬롯"에서 많은 MicroVM을 실행하고 다른 서비스는 고객이 Lambda를 호출할 때 MicroVM에서 실행되도록 코드를 예약합니다.
  
  2.  Frontend: The entrance into the Lambda system. It receives Invoke requests, and communicates with the Worker Manager to determine where to run the Lambda, then directly communicates with the Workers.
  * > Frontend: Lambda 시스템으로 들어가는 입구입니다. Invoke 요청을 수신하고 Worker Manager와 통신하여 Lambda를 실행할 위치를 결정한 다음 Worker와 직접 통신합니다.
  
  3.  Worker Manager: Ensures that the same Lambda is routed to the same set of Workers (this routing impacts performance for reasons that we will learn more about in the next section). It keeps tracks of where a Lambda has been scheduled previously. These previous runs correspond to “slots” for a function. If all of the slots for a function are in use, the Worker Manager works with the Placement service to find more slots in the Workers fleet.
  * > **Worker Manager: 동일한 Lambda가 동일한 작업자 세트로 라우팅되도록 합니다(이 라우팅은 다음 섹션에서 자세히 알아볼 이유로 성능에 영향을 미칩니다).** Lambda가 이전에 예약된 위치를 추적합니다. 이러한 이전 실행은 함수의 "슬롯"에 해당합니다. 기능에 대한 모든 슬롯이 사용 중인 경우 작업자 관리자는 배치 서비스와 협력하여 작업자 플릿에서 더 많은 슬롯을 찾습니다.

  4.  Placement service: Makes scheduling decisions when it needs to assign a Lambda invocation to a Worker. It makes these decision in order to “optimize the placement of slots for a single function across the worker fleet, ensuring that the utilization of resources including CPU, memory, network, and storage is even across the fleet and the potential for correlated resource allocation on each individual worker is minimized”
  * > Placement service: 작업자에게 Lambda 호출을 할당해야 하는 경우 일정 결정을 내립니다. 이러한 결정은 "작업자 플릿 전체에서 단일 기능에 대한 슬롯 배치를 최적화하여 CPU, 메모리, 네트워크 및 스토리지를 포함한 리소스 활용이 플릿 전체에서 균일하고 관련 리소스 할당 가능성이 있는지 확인합니다. 개별 작업자가 최소화됩니다.”

### Lambda worker architecture
* Each Lambda worker has thousands of individual MicroVMs that map to a “slot”.
> 각 Lambda 작업자에는 "슬롯"에 매핑되는 수천 개의 개별 MicroVM이 있습니다.

![image](https://user-images.githubusercontent.com/67637935/204988827-9da93881-9e6d-48b2-9dc9-8a1b5813afbd.png)

* Each MicroVM is associated with resource constraints (configured when a Lambda is setup) and communicates with several components that allow for scheduling, isolated execution, and teardown of customer code inside of a Lambda:
> 각 MicroVM은 리소스 제약 조건(Lambda가 설정될 때 구성됨)과 연결되며 예약, 격리된 실행 및 Lambda 내부의 고객 코드 분해를 허용하는 여러 구성 요소와 통신합니다.
  * Firecracker VM: All of the goodness we talked about earlier.
  * > Firecracker VM: 이전에 이야기한 모든 장점입니다.

  * Shim process: A process inside of the VM that communicates with an external side car called the Micro Manager.
  * > Shim 프로세스: Micro Manager라는 외부 사이드카와 통신하는 VM 내부의 프로세스입니다.

  * Micro Manager: a sidecar that communicates over TCP with a Shim process running inside the VM. It reports metadata that it receives back to the Placement service, and can be called by the Frontend in order to Invoke a specific function. On function completion, the Micro Manager also receives the response from the Shim process running inside the VM (passing it back to the client as needed).
  * > Micro Manager: VM 내부에서 실행되는 Shim 프로세스와 TCP를 통해 통신하는 사이드카입니다. Placement 서비스로 다시 수신한 메타데이터를 보고하고 특정 기능을 호출하기 위해 프런트엔드에서 호출할 수 있습니다. 기능 완료 시 Micro Manager는 VM 내에서 실행 중인 Shim 프로세스로부터 응답도 받습니다(필요에 따라 클라이언트에 다시 전달).

* While slots can be filled on demand, the Micro Manager also starts up Firecracker VMs in advance - this helps with performance (as we will see in the next section).
> 필요에 따라 슬롯을 채울 수 있지만 Micro Manager는 미리 Firecracker VM을 시작합니다. 이는 성능에 도움이 됩니다(다음 섹션에서 살펴보겠습니다).

### Performance

![image](https://user-images.githubusercontent.com/67637935/204991108-2192563f-c753-4b25-ae76-2ce13b4f7651.png)

* Firecracker was evaluated relative to similar VMM solutions on three dimensions: boot times, memory overhead, and IO Performance. In these tests, Firecracker was compared to QEMU and Intel Cloud Hypervisor
> Firecracker는 부팅 시간, 메모리 오버헤드 및 IO 성능이라는 세 가지 차원에서 유사한 VMM 솔루션과 비교하여 평가되었습니다. 이 테스트에서 Firecracker는 QEMU 및 인텔 클라우드 하이퍼바이저와 비교되었습니다.
* Additionally, there are two configurations of Firecracker used in the tests: Firecracker and Firecracker-pre.
> 또한 테스트에 사용된 Firecracker에는 Firecracker와 Firecracker-pre의 두 가지 구성이 있습니다.
* Because Firecracker MicroVMs are configured via API calls, the team tested setups where the API calls had completed (Firecracker-pre, where the “pre” means “pre-configured”) or had not completed (regular Firecracker). 
> Firecracker MicroVM은 API 호출을 통해 구성되기 때문에 팀은 API 호출이 완료되었거나(Firecracker-pre, 여기서 "사전"은 "사전 구성됨"을 의미함) 완료되지 않은(일반 Firecracker) 설정을 테스트했습니다.
* The timer for both of these configurations ended when the init process in the VM started.
> 이 두 구성 모두에 대한 타이머는 VM의 초기화 프로세스가 시작될 때 종료되었습니다.

#### Boot times
* 부팅 시간 비교에는 총 500개의 MicroVM을 직렬로 부팅하는 것과 한 번에 50개씩(병렬로) 총 1000개의 MicroVM을 부팅하는 두 가지 구성이 포함됩니다.

![image](https://user-images.githubusercontent.com/67637935/204991270-8e60d651-29ac-4775-9497-a3410c6226a9.png)

* 이러한 테스트의 결론은 Firecracker MicroVM이 매우 빠르게 부팅된다는 것입니다. 빠른 전환 ✅!

#### Memory overhead
* Firecracker는 다른 옵션에 비해 훨씬 적은 메모리(오버헤드 및 밀도)를 사용합니다.

![image](https://user-images.githubusercontent.com/67637935/204991407-eeb8f662-a758-4dd7-9642-a5847c2da502.png)

#### I/O Performance
* 다른 옵션에 비해 Firecracker와 이에 필적하는 인텔 클라우드 하이퍼바이저 솔루션은 모든 테스트에서 제대로 수행되지 않았습니다. 이 논문은 IO 테스트에서 상대적으로 열등한 성능의 원인이 디스크에 대한 플러시가 없고 IO를 직렬로 수행하는 블록 IO의 구현이라고 주장합니다. Firecracker에 대한 Github 문제를 파헤치면서 비동기 IO를 지원하고 IO 성능을 향상시키기 위해 io_uring을 사용하여 프로토타입을 만들고 있음을 나타내는 문제를 찾았습니다.

![image](https://user-images.githubusercontent.com/67637935/204991518-7752efd0-81c4-4a73-a6fb-fe35b072c179.png)

### Conclusion
Firecracker는 Rust로 작성된 고성능의 낮은 오버헤드 VMM이기 때문에 흥미로운 내용이었습니다. 또한 이 문서는 실용적인 기술 의사 결정에 대한 훌륭한 연구입니다. 팀은 이미 강력한 소프트웨어(KVM)를 다시 작성하는 대신 기존 시스템의 특정 구성 요소를 개선하는 데 집중했습니다. 그 과정에서 우리는 고객 워크로드를 서로 격리하는 다양한 방법에 대해 배웠습니다.
