---
tags: 🌟paper-review security Defensive-code-injection-attacks
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

# Introduction
* Definition of code-injection attack: Code-injection attacks are perhaps one of the most common attacks on modern computer systems. These attacks are used to deliver and run arbitrary code on victims’ machines, often enabling unauthorized access and control of system resources, applications, and data. Typically, the vulnerabilities
being exploited arise due to some level of neglect on the part of system and application developers to properly define and reject invalid program input. Indeed, the canonical consequences of such neglect, which include buffer and heap overflow attacks, format string attacks, and (more recently) heap spray attacks, categorically demonstrate some of the most popular code-injection techniques.
  * Generally speaking, an attacker’s first objective in a codeinjection attack is to gain control of a machine’s program counter.
  * The program counter is a special purpose machine register that identifies the next instruction scheduled for execution. By gaining control of the program counter, an attacker is able to redirect program execution and disruptthe intended behavior of the program.
  *  With the ability to manipulate the program counter, attackers sometimes redirect a victim’s machine to execute (already present) application or system code in a manner beneficial to an attacker’s intent.
  *  For instance, return-to-libc attacks provide a well-documented example of this kind of manipulation.
  *  In a code-injection attack, however, attackers redirect the program counter to execute code delivered by the attackers themselves. 
  *  Depending on the details of the particular vulnerability that an attacker is targeting,**injected code can take several forms including source code for an interpreted scripting-language engine, intermediate byte-code, or natively-executable machine code.**
*  Despite differences in the style and implementation of different exploits, e.g., buffer overflow versus format string attacks, all code-injection attacks share a common component: **the injected code.**
  *  **This payload, once executed after successful manipulation of the program counter, often provides attackers with arbitrary control over a vulnerable machine.**
  * **_Frequently (though not always), attackers deliver a payload that simply launches a command shell. It is for this reason that many in the hacking community generically refer to the payload portion of a code-injection attack as shellcode._**
* Among those less familiar with code-injection attacks, there is sometimes a subtle misconception that shellcode is necessarily delivered in tandem with whichever message ultimately exploits a vulnerability and grants an attacker control of the program counter.
  * While this assumption typically holds in more traditional buffer overflow vulnerabilities, modern attacks demonstrate that attackers have developed numerous techniques to covertly deliver (and ultimately store into memory) shellcode separately from and prior to triggering the exploit. 
  * For instance, if an attacker can manipulate memory at a known heap address, they may store their shellcode there, using its address later when overwriting a return address on the stack.
  * **We draw attention to this distinction because our use of the term shellcode in this paper specifically denotes the injected code irrespective of individual attacks or vulnerabilities.**
* Typically, shellcode takes the form of directly executable machine code, and consequently, several defensive measures that attempt to detect its presence, or prevent its execution altogether, have been proposed.
  * Indeed, automated inspection of user input, system memory, or network traffic for content that appears statistically or superficially executable are now common.
  * However, as expected, a number of techniques have been developed that circumvent these protective measures, or make their job far more difficult (e.g., polymorphism).
* **Recently, it has been suggested that even polymorphic shellcode is constrained by an essential component: the decoder.**
  * **The argument is that the decoder is a necessary and executable companion to encoded shellcode, enabling the encoded portion of the payload to undergo an inverse transformation to its original and executable form.**
  * **Since the decoder must be natively executable, the prevailing thought is that we can detect its presence assuming that this portion of the payload will bear some identifiable features not common to valid or non-executable data. **
  * It is this assumption **that shellcode is fundamentally different in structure than non-executable payload data—that continues to drive some avenues of contemporary research** (e.g., [27, 16, 15, 26]).
* By challenging the assumption that shellcode must conform to superficial and discernible representations, we question whether protective measures designed to assume otherwise are likely to succeed. 
  * **Specifically, we demonstrate a technique for automatically producing English Shellcode that is, transforming arbitrary shellcode into a representation that is statistically similar to English prose.**
  *  **By augmenting corpora-based natural-language generation with additional constraints uniquely dictated by each instance of shellcode, we generate encodings complete with decoder that remain statistically faithful to the corpus and yield identical execution as the original shellcode.**
  *  While we in no way claim that instantiations of this encoding are irrefutably indistinguishable from authentic English prose—indeed, as shown later, it is clear they are not—the expected burden associated with reliably detecting English-encoded shellcode variants in juxtaposition to genuine payloads at line speed raises concerns about current preventative approaches.
*  Similar to the goal of Song et al. [20], our objective in this paper is to promote discussion and stimulate new ideas for thinking about **how to tackle evolutions in code-injection attacks.**
  * Although most of the attacks observed today have used relatively na¨ıve shellcode engines [17, 26], exploit code will likely continue to evade intrusion detection and prevention systems because malcode developers do not follow the “rules”. 
  * As this cat and mouse game plays on, it is clear that the attackers will adapt.
  * So should we, especially as it pertains to exploring new directions for preventative measures against code-injection attacks.

# On the ARMS RACE
* In this paper, we focus on natively-executable shellcode for x86 processors. 
* In this case, machine code and shellcode are fundamentally identical; they both adhere to the same binary representation directly executable by the processor.
* Shellcode developers are often faced with constraints that **limit the range of byte-values accepted by a vulnerable application.**
  * For instance, many applications restrict input to certain character-sets (e.g., printable, alphanumeric, MIME), or filter input with common library routines like isalnum and strspn.
* The difficulty in overcoming these restrictions and bypassing input filters depends on the range of acceptable input.
  * Of course, these restrictions can be bypassed by writing shellcode that does not contain restricted bytevalues (e.g., null-bytes).
  * Although such restrictions often limit the set of operations available for use in an attack, attackers have derived encodings to convert unconstrained shellcode honoring these restrictions by building equivalency operations from reduced instruction sets (e.g., [25, 11]).
    * Of special note are the alphanumeric encoding engines [18] present in Metasploit (see www.metasploit.com). 
    * **These engines convert arbitrary payloads to representations composed only of letters and numerical digits.(이 엔진은 임의의 페이로드를 문자와 숫자로만 구성된 표현으로 변환합니다.)** 
  * These encodings are significant for two reasons.
    * First, alphanumeric shellcode can be stored in atypical and otherwise unsuspected contexts such as syntactically valid file and directory names or user passwords (영숫자 쉘코드는 구문적으로 유효한 파일 및 디렉토리 이름 또는 사용자 암호와 같은 비정형적이거나 의심되지 않는 컨텍스트에 저장할 수 있습니다.). 
    * Second, the alphanumeric character set is significantly smaller than the set of characters available in Unicode and UTF-8 encodings( 영숫자 문자 집합은 유니코드 및 UTF-8 인코딩에서 사용할 수 있는 문자 집합보다 상당히 작습니다.).
  * This means that **the set of instructions available for composing alphanumeric shellcode is relatively small. **
  * To cope with these restrictions, patching or self-modification is often used. 
  * **Since alphanumeric engines produce encodings automatically, a decoder is required.**
  * **The challenge then is to develop an encoding scheme and decoder that use only alphanumeric characters (and hence, a restricted instruction set), yet are together capable of encoding arbitrary payloads.**
* The top three rows in Figure 1 show examples using the Metasploit framework.

![image](https://user-images.githubusercontent.com/67637935/201266401-bc386478-bae9-4fd4-9f40-be28fa38708d.png) 
> Example encodings of a Linux IA32 Bind Shell. The PexAlphaNum and Alpha2 encodings were generated using the Metasploit Framework. A hash symbol in the last column represents a character that is either unprintable or from the extended ASCII character set.

* We note that much of the literature describing code injection attacks (and prevention) assumes a standard attack template consisting of the basic components found traditionally in buffer-overflow attacks: a NOP sled, shellcode, and one or more pointers to the shellcode [1, 12, 23, 27]. 
  * Not surprisingly, the natural reaction has been to develop techniques that detect such structure or behavior [20, 23, 16, 15, 27, 14]. While emulation and static analysis have been successful in identifying some of the failings of advanced shellcode, in the limit, the overhead will likely make doing so improbable.
  * Moreover, attacks are not constrained to this layout and so attempts at merely detecting this structure can be problematic; infact, identifying each component has its own unique set of challenges [1, 13], and it has been suggested that malicious polymorphic behavior cannot be modeled effectively [20]. 
  * In support of that argument, we provide a concrete instantiation that shows that the decoder can share the same properties as benign data.

# Related Work 
(분류를 소개하고, 세부적인 관련 연구들을 소개한다-->괜찮은듯)
* Three types of defensive approaches about code-injection attacks
  1. **Tools and techniques to both limit the spoils of exploitations and to prevent developers from writing vulnerable code.**
    * Examples of such approaches include automatic bounds protection for buffers [4] and static checking of format strings at compile-time, utilizing “safe” versions of system libraries, and address-space layout randomization [19], etc
    * While these techniques reduce the attack surface for code-injection attacks, no combination of such techniques seems to systematically eliminate the threat of code-injection [6, 21].
  2. **In light of persistent vulnerabilities, the second category of countermeasures focuses on preventing the execution of injected code.**
    * In this realm, researchers have demonstrated some success using methods that randomize the instructionset [22] or render portions of the stack non-executable.
    * Although these approaches can be effective, instruction-set randomization is considered too inefficient for some workloads. 
    * Additionally, recent work by Buchanan et al. demonstrates that without better support for constraining program behavior, execution-redirection attacks are still possible [3].  
  3. The third category for code-injection defense **consists of content-based input-validation techniques.**
    * These approaches are either host or network-based and are typically used as components in intrusion detection systems. 
    * User-input or network traffic is considered suspicious when it appears executable or anomalous as determined by heuristic, signature, or simulation.
    * In this area, Toth and Kruegel detect some buffer overflow exploits by **interpreting network payload**s as executable code and analyzing their execution structure [23]. They divide machine instructions into two categories separated by those that modify the program counter, i.e., jump instructions, and others that do not. Their experiments show that, under some circumstances, it is possible to identify payloads with executable code by evaluating the maximum length of instruction
sequences that fall between jump instructions, and find that payloads with lower maximum execution lengths are typically benign. However, their evaluation does not include an analysis of polymorphic code, and Kolesnikov et al. show that polymorphic blending attacks evade this detection approach [9].
* Lastly, Song et al. examine popular polymorphic shellcode engines to assess their strengths and weaknesses [20]. 
  * Our work supports their observations in that while today’s polymorphic engines do generate observable artifacts, these artifacts are not intrinsically symptomatic of polymorphic code.
  * **However, while they advise that modeling acceptable content or behavior may lead to a better long-term solution for preventing shellcode delivery, we argue that even modeling acceptable content will be rife with its own set of challenges, as exemplified by English shellcode.**
  * Specifically, by generating malicious code that draws from a language model built using only benign content, statistical measures of intent become less accurate and the signal-to-noise ratio between malicious code and valid network data declines.

# Towards English shellcode
* Shellcode, like other compiled code, is simply an ordered list of machine instructions. 
  * At the lowest level of representation, each instruction is stored as a series of bytes signifying a pattern of signals that instruct the CPU to manipulate data as desired.
  * Like machine instructions, non-executable data is represented in byte form.
  * Coincidentally, some character strings from the ASCII character and native machine instructions have identical byte representations.
  * Moreover, it is even possible to find examples of this phenomenon that parse as grammatically correct English sentences.
    * For instance, ASCII representation of the phrase “Shake Shake Shake!” is byte-equivalent to the following sequence of Intel instructions: push %ebx; push "ake "; push %ebx; push "ake "; push %ebx; push "ake!".
  * However, it is unlikely that one could construct meaningful code by simply concatenating English phrases that exhibit this property.
  * Abiding by the rules of English grammar simply excludes the presence of many instructions and significantly limits the availability and placement of others.
    * For example, add, mov, and call instructions cannot be constructed using this method.
  * Therefore, while it may be possible to construct some instances of shellcode with coherent objectives in this manner, the versatility of this technique is severely restricted by its limitations.
  * **Rather than find these instances, our goal is instead to develop an automated approach for transforming arbitrary shellcode into an English representation.**

## High-level Overview
* What follows is a brief description of the method we have developed for encoding arbitrary shellcode as English text.
* This English shellcode is completely self-contained, i.e., it does not require an external loader, and executes as valid IA32 code. 
*  The steps depicted in Figure 3 complement the brief overview of our approach presented below.
![image](https://user-images.githubusercontent.com/67637935/201276307-ede8113a-42f1-4793-84ce-84b034e79d55.png)

  1) English-Compatible Decoder
     * Write a decoder that is capable of encoding generic payloads using only English-compatible instructions.  
  2) Language Model Generation
     * Generate and train a natural language model with a large and diverse corpus of English text. 
  3) Viterbi Search and Execution
     * Using Viterbi search, traverse the language model, executing and scoring each candidate decoder.
  4) Encode Target Shellcode
     * Continue to traverse the language model, encoding the target shellcode as English. Upon delivery, this code will be decoded and executed. 


*  One can envision a typical usage scenario (see Figure 4) where the English shellcode (composed of a natively executable decoder and an encoded payload containing arbitrary shellcode) is first generated offline.

![image](https://user-images.githubusercontent.com/67637935/201276365-a2b1c98d-cfd3-4e77-b48f-37e627d91ebd.png)

*  Once the English shellcode is delivered to a vulnerable machine and its vulnerability is triggered, execution is redirected to the English shellcode, initiating the decoding process and launching the target shellcode contained in the payload.
