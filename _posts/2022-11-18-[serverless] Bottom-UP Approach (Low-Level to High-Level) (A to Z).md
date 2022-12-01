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
