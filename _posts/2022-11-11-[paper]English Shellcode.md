---
tags: 🌟paper-review security cloud
toc : True
---

# Abstract
* History indicates that the security community commonly takes a divide-and-conquer approach to battling malware threats: identify the essential and inalienable components of an attack, then develop detection and prevention techniques that directly target one or more of the essential components.
> 역사에 따르면 보안 커뮤니티는 일반적으로 맬웨어 위협과 싸우기 위해 분할 정복 방식을 취합니다. 공격의 필수 구성 요소와 양도할 수 없는 구성 요소를 식별한 다음, 필수 구성 요소 중 하나 이상을 직접 대상으로 하는 탐지 및 방지 기술을 개발합니다.
* This abstraction is evident in much of the literature for buffer overflow attacks including, for instance, stack protection and NOP sled detection. It comes as no surprise then that we approach shellcode detection and prevention in a similar fashion.
> 이 추상화는 예를 들어 스택 보호 및 NOP 슬레드 감지를 포함한 버퍼 오버플로 공격에 대한 많은 문헌에서 분명합니다. 우리가 비슷한 방식으로 쉘코드 탐지 및 방지에 접근한다는 것은 놀라운 일이 아닙니다.
* However, the common belief that components of polymorphic shellcode (e.g., the decoder) cannot reliably be hidden suggests a more implicit and broader assumption that continues to drive contemporary research: namely, that valid and complete representations of shellcode are fundamentally different in structure than benign payloads.
> 그러나, 다형 셸 코드의 구성 요소(예: 디코더)를 안정적으로 숨길 수 없다는 일반적인 믿음은 현대 연구를 계속 추진하는 더 암묵적이고 광범위한 가정을 제시한다. 즉, 셸 코드의 유효하고 완전한 표현은 양성 페이로드와 구조가 근본적으로 다르다는 것이다.
* While the first tenet of this assumption is philosophically undeniable (i.e., a string of bytes is either shellcode or it is not), truth of the latter claim is less obvious if there exist encoding techniques capable of producing shellcode with features nearly indistinguishable from non-executable content.
> 이 가정의 첫 번째 원칙은 철학적으로 부인할 수 없지만(즉, 바이트의 문자열은 셸 코드이거나 그렇지 않음), 실행 불가능한 내용과 거의 구별할 수 없는 특징을 가진 셸 코드를 생성할 수 있는 인코딩 기술이 존재한다면 후자의 주장의 진실은 덜 명백하다.
* In this paper, we challenge the assumption that shellcode must conform to superficial and discernible representations.
> 본 논문에서, 우리는 셸 코드가 피상적이고 식별 가능한 표현을 준수해야 한다는 가정에 도전한다.
* Specifically, we demonstrate a technique for automatically producing English Shellcode, transforming arbitrary shellcode into a representation that is superficially similar to English prose.
> __특히, 우리는 임의의 셸 코드를 표면적으로 영어 산문과 유사한 표현으로 변환하는 영어 셸 코드를 자동으로 생성하는 기술을 시연한다.__
* The shellcode is completely self-contained i.e., it does not require an external loader and executes as valid IA32 code) and can typically be generated in under
an hour on commodity hardware.
> 쉘코드는 완벽히 sellf-contained 되어져있고 ( 즉, IA32에 타당한 실행이나 외부 로더를 요구하지 않는다) 그리고 전형적으로 한시간안에 발생되어진다.
* Our primary objective in this paper is to promote discussion and stimulate new ideas for thinking ahead about preventive measures for tackling evolutions in code-injection attacks.
> 이 논문에서 우리의 주된 목적은 토론을 촉진하고 코드 주입 공격에서 진화를 다루기 위한 예방 조치에 대해 미리 생각할 수 있는 새로운 아이디어를 자극하는 것이다.

# Conclusion
* In this paper we revisit the assumption that shellcode need be fundamentally different in structure than non-executable data.
> 이 논문에서 우리는 셸 코드가 실행 불가능한 데이터와 근본적으로 다른 구조가 필요하다는 가정을 다시 살펴본다.
* Specifically, we elucidate how one can use natural language generation techniques to produce shellcode that is superficially similar to English prose.
> 구체적으로, 우리는 어떻게 자연어 생성 기술을 사용하여 표면적으로 영어 산문과 유사한 쉘코드를 생성할 수 있는지를 설명한다.
* We argue that this new development poses significant challenges for inline payloadbased inspection (and emulation) as a defensive measure, and also highlights the need for designing more efficient techniques for preventing shellcode injection attacks altogether.
> 우리는 이 새로운 개발이 방어 수단으로서 인라인 페이로드 기반 검사(및 에뮬레이션)에 중요한 과제를 제기하고, 또한 셸 코드 주입 공격을 완전히 방지하기 위한 보다 효율적인 기술을 설계해야 할 필요성을 강조한다고 주장한다.
