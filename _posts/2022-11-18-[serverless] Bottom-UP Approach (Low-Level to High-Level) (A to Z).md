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

