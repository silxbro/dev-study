## [2-3] CPU

컴퓨터에서 가장 중요한 부품 중 하나를 꼽으라고 한다면 단연 CPU를 꼽는 사람들이 많을 것이다.
그만큼 CPU는 중요한 부품이지만, 중요하다고 해서 CPU의 내부 회로나 작동 방식 하나 하나를 모두 알아야 하는 것은 아니다.
이번 절에서는 개발자가 알아야 할 배경지식에 초점을 맞춰 CPU에 대해 알아보자.
<br/>
<br/>
### 🍓 레지스터
레지스터는 CPU 안에 있는 작은 임시 저장장치이다. CPU 안에는 다양한 레지스터들이 있고, 각기 다른 **이름과 역할**이 있다.
프로그램을 이루는 데이터와 명령어가 프로그램의 실행 전후로 레지스터에 저장되기 때문에 레지스터에 저장된 값만 잘 관찰해도 비교적 낮은 수준의 프로그램이 어떻게 작동하는지를 파악할 수 있다.

레지스터는 다음과 같은 WinDbg(윈도우 운영체제), gdb(리눅스, 맥OS 운영체제) 등의 디버깅 도구를 이용해 관찰할 수 있다.

<img src="https://github.com/user-attachments/assets/a6e6451b-718e-433d-865f-29189f907480" width="600"/><br/>

CPU를 구성하는 레지스터들의 세세한 종류와 이름은 조금씩 다르지만, 앞으로 설명할 레지스터들은 **대부분의 CPU가 공통적으로 포함하고 있는 대표적인 레지스터**이다.
모두 저마다의 역할이 있어 각자의 역할에 걸맞은 내용들을 저장한다. 그럼 **각각의 레지스터가 CPU 내부에서 어떤 역할을 수행하는지**에 유의하며 살펴보자.

#### [1] 프로그램 카운터
**프로그램 카운터(PC, Program Counter)** 는 **메모리에서 다음으로 읽어 들일 명령어의 주소**를 저장한다.
프로그램 카운터를 **명령어 포인터(IP, Instruction Pointer)** 라고 부르는 CPU도 있다.
일반적으로 프로그램 카운터는 1씩 증가하는데, 이는 곧 다음으로 읽어 들일 메모리 주소가 1씩 증가하는 것과 같다.
메모리에 저장된 프로그램이 순차적으로 실행될 수 있는 것은 근본적으로 프로그램 카운터 값이 1씩 증가하며 실행되기 때문이다.

<img src="https://github.com/user-attachments/assets/b9cf39ed-2214-4940-a61c-e2fefd80ab07" width="550"/><br/>

다만, 프로그램 카운터가 언제나 증가만 하는 것은 아니다. 프로그래밍 언어의 조건문이나 리턴문을 생각해보자. 조건이 참이 되거나 리턴문을 실행하는 경우 프로그램이 순차적으로 실행되지 않는다.
이렇게 프로그램의 실행 흐름이 순차적이지 않을 때는 프로그램 카운터 값이 임의의 위치로 변경된다.

<img src="https://github.com/user-attachments/assets/8515cf15-2b76-4aee-a57b-86807bdf733e" width="580"/><br/>

#### [2] 명령어 레지스터
**명령어 레지스터(IR, Instruction Register)** 는 **해석할 명령어**, 즉 메모리에서 방금 읽어 들인 명령어를 저장하는 레지스터이다.
CPU 내의 제어장치는 명령어 레지스터 속 명령어를 해석한 뒤 ALU(산술논리연산장치)로 하여금 연산하도록 시키거나 다른 부품으로 제어 신호를 보내 해당 부품을 작동시킨다.

#### [3] 범용 레지스터
**범용 레지스터(general purpose register)** 는 이름 그대로 다양하고 일반적인 상황에서 자유롭게 사용할 수 있는 레지스터이다. 데이터와 명령어, 주소 모두를 저장할 수 있다.
일반적으로 CPU 안에는 여러 개의 범용 레지스터들이 있다.

#### [4] 플래그 레지스터
**플래그 레지스터(flag register)** 는 연산의 결과 혹은 CPU 상태에 대한 부가 정보인 **플래그(flag)** 값을 저장하는 레지스터이다.
플래그는 CPU가 명령어를 처리하는 과정에서 반드시 참조해야 할 상태 정보를 의미하는 비트이다. 대표적인 플래그의 종류는 다음과 같다. 플래그 레지스터는 다음과 같은 플래그(비트)들로 구성되어 있는 셈이다.

<img src="https://github.com/user-attachments/assets/1a99f778-ab44-4dd5-9500-51ce5ebabd33" width="650"/><br/>

예를 들어 다음과 같은 플래그 레지스터에서 CPU가 연산을 수행한 직후에 부호 플래그가 1이 되었다면 이는 연산의 결과가 음수임을 나타낸다.

<img src="https://github.com/user-attachments/assets/27f46938-ba8c-4a36-a948-17d9e1591d02" width="400"/><br/>

#### [5] 스택 포인터
메모리에는 실행 중인 프로그램들이 적재되어 있으며, 실행 중인 각 프로그램들은 스택과 같은 형태로 사용 가능한 주소 공간을 하나 이상 가지고 있다.
암묵적으로 스택처럼 사용하자고 약속된 이 메모리 영역을 스택 영역이라고 한다. **스택 포인터(stack pointer)** 란 메모리 내 스택 영역의 최상단 스택 데이터 위치를 가리키는 특별한 레지스터를 말한다.

예를 들어, 다음 그림의 CPU에 있는 스택 포인터는 '스택 데이터 3'이 저장된 공간을 가리키고 있다.
만약 이 스택에서 데이터를 꺼낸다면 가장 꼭대기에 있는 '스택 데이터 3'을 꺼내고, 스택 포인터는 '스택 데이터 2'가 저장된 주소를 가리키게 된다.
스택 포인터는 마지막으로 스택에 저장된 데이터의 위치를 가리키는 레지스터이자, 스택이 채워진 정도를 나타내는 레지스터인 셈이다.

<img src="https://github.com/user-attachments/assets/152b7723-31a9-45b2-9923-c3156c8ca053" width="450"/><br/>
<br/>

### 🍓 인터럽트
명령어 사이클을 학습하면서 CPU의 레지스터를 알아야만 이해할 수 있는 인터럽트 사이클에 대해 언급했다. 인터럽트라는 용어는 프로그래밍 언어로 개발해 본 사람이라면 한 번쯤 접해 본 적이 있을 것이다.
인터럽트는 다양한 상황에서 발생할 수 있는데, 임의로 발생시킬 수도 있고, 잘못된 프로그램으로 인해 발생하기도 하며, 효율적인 입출력을 위해 사용되기도 한다.
인터럽트는 컴퓨터 구조는 물론, 운영체제를 이해하는 데에도 아주 중요한 개념이므로 집중해서 파악해 두기를 바란다.

<img src="https://github.com/user-attachments/assets/8838e0e8-08d4-4fe2-a18e-57ac131d9767" width="600"/><br/>

인터럽트(interrupt)는 '방해하다, 중단시키다'라는 의미이다. CPU가 수행 중인 작업은 방해를 받아 잠시 중단될 수 있는데, 이렇게 **CPU의 작업을 방해하는 신호**를 **인터럽트(interrupt)** 라고 한다.
인터럽트의 종류를 살펴보면 인터럽트가 발생하는 상황을 엿볼 수 있다.

인터럽트는 크게 **동기 인터럽트와 비동기 인터럽트**로 나뉜다. **동기 인터럽트(synchronous interrupts)** 는 CPU에 의해 발생하는 인터럽트이다.
가령 CPU가 프로그래밍 오류와 같은 **예외적인 상황**(예상치 못한 상황)을 마주쳤을 때 발생하는 인터럽트다. 이런 점에서 동기 인터럽트는 **예외(exception)** 라고도 부른다.

**비동기 인터럽트(asynchronous interrupts)** 는 주로 **입출력장치에 의해 발생**하는 인터럽트이다.
이는 세탁기의 세탁 완료 알림, 전자레인지의 조리 완료 알림과 같은 **알림**의 역할을 하며, 다음과 같이 활용된다.
- CPU가 프린터와 같은 입출력장치에게 입출력 작업을 부탁하고, 작업을 끝낸 입출력장치가 CPU에게 **완료 알림(인터럽트)** 을 보낸다.
- 키보드, 바우스와 같은 입출력장치가 어떤 입력을 받아들였을 때, 이를 처리하기 위해 CPU에게 **입력 알림(인터럽트)** 을 보낸다.

일반적으로 비동기 인터럽트를 인터럽트라고 지칭하기도 하지만, 용어의 혼동을 방지하기 위해 **하드웨어 인터럽트**라는 용어를 사용한다.
<br/>
#### [하드웨어 인터럽트]
하드웨어 인터럽트는 알림과 같은 인터럽트라고 했다. CPU는 **효율적으로 명령어를 처리하기 위해** 하드웨어 인터럽트를 사용한다. 명령어를 효율적으로 처리하는 것과 하드웨어 인터럽트는 어떤 상관이 있을까?

가령 CPU가 프린터에 프린트를 명령했다고 가정해 보자. 일반적으로 입출력장치의 속도는 CPU에 비해 현저히 느리다. 그래서 CPU는 입출력 작업의 결과를 바로 받아 볼 수 없다.
만약 하드웨어 인터럽트를 사용하지 않는다면 CPU는 프린터가 언제 프린트를 끝낼지 모르기 때문에 주기적으로 프린터의 완료 여부를 확인해야 한다.
이거은 마치 알림이 없는 전자레인지 앞에서 언제 조리가 끝날지 무작정 기다리는 상황과 같다.

> 작업 완료 여부를 계속해서 확인하는 것은 인터럽트와 대비되는 '폴링'이라는 기법이다.
> 입출력 작업에서 **폴링(polling)** 이란 입출력장치의 상태가 어떤지, 처리할 데이터가 있는지 **주기적으로 확인하는 것을 말한다.**

하지만 하드웨어 인터럽트를 사용하면 CPU는 작업이 끝나기를 마냥 기다릴 필요 없이 프린트가 진행되는 동안 온전히 다른 작업을 처리할 수 있게 된다.
이렇듯 하드웨어 인터럽트는 입출력 완료 여부를 확인하기 위한 CPU 사이클 낭비를 최소화하고, CPU가 다른 일을 수행할 수 있는 시간을 벌어 줌으로써 효율적으로 명령어를 처리할 수 있도록 돕는다.
그럼 CPU가 하드웨어 인터럽트를 어떻게 처리하는지 구체적으로 알아보자.

여러 종류의 인터럽트가 있기는 하지만, CPU가 인터럽트를 처리하는 방식은 인터럽트의 종류를 막론하고 대동소이하다. 일반적으로 **CPU가 하드웨어 인터럽트를 처리하는 순서**는 다음과 같다.
강조하여 표기한 용어가 이번 절에서 기억해야 할 키워드이다.

- [1] 입출력 장치는 CPU에게 **인터럽트 요청 신호**를 보낸다.
- [2] CPU는 실행 사이클이 끝나고 명령어를 인출하기 전에 항상 인터럽트 여부를 확인한다.
- [3] CPU는 인터럽트 요청을 확인하고, **인터럽트 플래그**를 통해 현재 인터럽트를 받아들일 수 있는지 여부를 확인한다.
- [4] 인터럽트를 받아들일 수 있다면 CPU가 지금까지의 작업을 백업하다.
- [5] CPU는 **인터럽트 벡터**를 참조하여 **인터럽트 서비스 루틴**을 실행한다.
- [6] 인터럽트 서비스 루틴 실행이 끝나면 [4]에서 백업해 둔 작업을 복구하여 실행을 재개한다.

인터럽트와 관련해 알아야 할 용어는 인터럽트 요청 신호, 인터럽트 플래그, 인터럽트 벡터, 인터럽트 서비스 루틴이다. 그럼 하나씩 알아보자.
인터럽트는 CPU의 정상적인 실행 흐름을 끊는 것이기 때문에 인터럽트하기 전에 CPU에게 인터럽트의 가능 여부를 확인해야 한다. 이를 위한 신호를 **인터럽트 요청(interrupt request) 신호**라고 한다.

<img src="https://github.com/user-attachments/assets/359895c6-b025-4914-b33b-f37f081c7f2c" width="370"/><br/>

이때 CPU가 인터럽트 요청을 수용하기 위해서는 플래그 레지스터의 **인터럽트 플래그(interrupt flag)** 가 활성화되어 있어야 한다.
인터럽트 플래그는 하드웨어 인터럽트를 받아들일지, 무시할지를 결정하는 플래그이다. 만약 인터럽트 플래그가 불가능으로 설정되어 있다면 CPU는 인터럽트 요청이 오더라도 해당 요청을 무시한다.

다만, 모든 하드웨어 인터럽트를 인터럽트 플래그로 막을 수 있는 것은 아니다. 인터럽트 플래그가 불가능으로 설정되어 있더라도 무시할 수 없는 인터럽트 요청도 있다.
무시할 수 없는 하드웨어 인터럽트는 가장 우선순위가 높은, 다시 말해 가장 먼저 처리해야 하는 인터럽트를 말한다. 정전이나 하드웨어 고장으로 인한 인터럽트가 이에 해당한다.
즉, 하드웨어 인터럽트에는 인터럽트 플래그로 **막을 수 있는 인터럽트(maskable interrupt)** 와 **막을 수 없는 인터럽트(non maskable interrupt)** 가 있다.
<br/>
<br/>
<img src="https://github.com/user-attachments/assets/be4aaba8-78d3-4f6f-9baa-9ad3276398bd" width="420"/><br/>

> 막을 수 없는 인터럽트는 NMI(Non Maskable Interrupt)라고도 부른다.

CPU가 인터럽트 요청을 받아들이기로 했다면 CPU는 인터럽트 서비스 루틴이라는 프로그램을 실행한다.
**인터럽트 서비스 루틴(ISE, Interrupt Service Routine)** 은 인터럽트를 처리하기 위한 프로그램으로, **인터럽트 핸들러(interrupt handler)** 라고도 부른다.
인터럽트 서비스 루틴은 **어떤 인터럽트가 발생했을 때 해당 인터럽트를 어떻게 처리하고 작동해야 할지에 대한 정보**로 이루어진 프로그램이다.
요컨대 CPU가 인터럽트를 처리한다는 말은 **인터럽트 서비스 루틴을 실행하고, 본래 수행하던 작업으로 다시 되돌아온다**는 말과 같다.

<img src="https://github.com/user-attachments/assets/47eeec95-4309-40cf-ad17-53f2cbf05d92" width="580"/><br/>

여기까지 보면 인터럽트 서비스 루틴의 과정이 일반적인 함수의 실행 과정과 똑같다. 실제로 인터럽트 서비스 루틴을 실행하는 것은 본질적으로 함수를 실행하는 것과 다르지 않다.
실행의 조건이 충족되면 실행되는 함수와 같다.

입출력장치마다 인터럽트를 처리하기 위한 동작이 다르므로 입출력장치마다 각기 다른 인터럽트 서비스 루틴을 가지고 있다.
즉, 메모리에는 여러 개의 인터럽트 서비스 루틴들이 저장되어 있고, 이들 하나 하나가 **'인터럽트가 발생하면 어떻게 행동해야 할지'를 알려 주는 프로그램**이라고 보면 된다.

그렇다면 CPU가 각기 다른 인터럽트 서비스 루틴들을 구분할 수 있어야 하는데, CPU는 수많은 인터럽트 서비스 루틴들을 구분하기 위해 인터럽트 벡터를 이용한다.
**인터럽트 벡터(interrupt vector)** 는 인터럽트 서비스 루틴을 식별하기 위한 정보이다. CPU는 하드웨어 인터럽트 요청을 보낸 대상으로부터 버스를 통해 인터럽트 벡터를 전달받는다.
인터럽트 벡터가 인터럽트 서비스 루틴의 시작 주소를 포함하고 있기 때문에 CPU는 인터럽트 벡터를 통해 처음부터 특정 인터럽트 서비스 루틴을 실행할 수 있다.
가령 다음 그림에 있는 인터럽트 서비스 루틴 B가 프린터의 인터럽트 서비스 루틴이라고 가정해 보자.

<img src="https://github.com/user-attachments/assets/535fbf43-2ced-4c15-a266-d97dd81cc400" width="500"/><br/>

CPU가 작업을 수행하는 도중에 프린터 인터럽트가 발생한 경우, CPU는 전달받은 인터럽트 벡터를 참조하여 프린터 인터럽트 서비스 루틴의 시작 주소(인터럽트 서비스 루틴 B의 시작 주소)를 알아내고,
이 시작 주소부터 실행하면서 프린터의 인터럽트 서비스 루틴을 실행한다.

정리하자면, CPU가 인터럽트를 처리하는 것은 **인터럽트 서비스 루틴을 실행하고, 본래 수행하던 작업으로 다시 되돌아온다**는 말과 같다.
그리고 CPU가 인터럽트 서비스 루틴을 실행하려면 **인터럽트 서비스 루틴의 시작 주소**를 알아야 하며, 이는 **인터럽트 벡터**를 통해 알 수 있다.

여느 프로그램과 마찬가지로 인터럽트 서비스 루틴 또한 데이터와 명령어로 이루어져 있기 때문에 인터럽트 서비스 루틴도 프로그램 카운터를 비롯한 레지스터들을 사용하면서 실행된다.
이때 인터럽트 요청을 받기 직전까지 CPU가 수행하고 있었던 일은 인터럽트 서비스 루틴이 끝나면 되돌아와서 마저 수행되어야 하므로 지금까지의 작업들을 어딘가에 **백업**해 둬야 한다.
따라서 CPU는 인터럽트 서비스 루틴을 실행하기 전에 프로그램 카운터 값 등 **현재 프로그램을 재개하기 위해 필요한 모든 내용**을 메모리 내 **스택**에 백업한다.

그리고 나서 인터럽트 서비스 루틴의 시작 주소가 위치한 곳으로 프로그램 카운터 값을 갱신하고, 인터럽트 서비스 루틴을 실행한다.
인터럽트 서비스 루틴을 모두 실행하면, 다시 말해 인터럽트를 처리하고 나면 스택에 저장해 둔 프로그램 카운터 등을 다시 불러온 뒤 이전까지 수행하던 작업을 재개한다.

<img src="https://github.com/user-attachments/assets/9a2e0425-fc2f-44d4-a55c-873d588ae59a" width="520"/><br/>
<br/>

지금까지 CPU가 인터럽트를 처리하는 과정을 알아보았다. **인터럽트를 처리하는 인터럽트 사이클**까지 추가한 명령어 사이클의 모습은 다음과 같다.
결국 CPU는 이와 같은 과정을 반복하며 프로그램을 실행해 나간다고 할 수 있다.

<img src="https://github.com/user-attachments/assets/3d6824f9-47f4-4f1e-ae4b-0bf21507ca78" width="470"/><br/>
<br/>
#### [예외]
예외(동기 인터럽트)의 종류에는 폴트, 트랩, 중단, 소프트웨어 인터럽트 등이 있다. CPU는 예외가 발생하면 하던 일을 중단하고 해당 예외를 처리한다.
그리고 예외를 처리하고 나면 다시 본래 하던 작업으로 되돌아와 실행을 재개한다.
CPU가 본래 하던 작업으로 되돌아왔을 때 **예외가 발생한 명령어**부터 실행하느냐, **예외가 발생한 명령어의 다음 명령어**부터 실행하느냐에 따라 폴트와 트랩으로 나눌 수 있다.

<img src="https://github.com/user-attachments/assets/eb48b392-2a04-4962-983d-d089ba311910" width="450"/><br/>

**폴트(falut)** 는 예외를 처리한 직후에 **예외가 발생한 명령어부터** 실행을 재개하는 예외이다.
가령 CPU가 명령어를 실행하려고 할 때, 명령어 실행을 위해 꼭 필요한 데이터가 메모리가 아닌 보조기억장치에 저장되어 있다고 가정해 보자.
프로그램이 실행되기 위해서는 데이터가 반드시 메모리에 저장되어 있어야 하므로 CPU는 폴트를 발생시키고, 보조기억장치로부터 필요한 데이터를 메모리로 가져와 저장하게 된다.
CPU는 필요한 데이터를 보조기억장치로부터 메모리로 가지고 왔으므로 다시 실행을 재개해 **폴트가 발생한 그 명령어부터** 실행해 나갈 것이다(이를 페이지 폴트라고 한다).
이처럼 예외가 발생한 명령어부터 실행해 나가는 예외를 폴트라고 한다.

<img src="https://github.com/user-attachments/assets/342a651c-e314-4ba2-abf5-5de76b07d7da" width="420"/><br/>
<br/>
**트랩(trap)** 은 예외를 처리한 직후에 **예외가 발생한 명령어의 다음 명령어부터** 실행을 재개하는 예외이다. 대표적인 사례로 디버깅의 브레이크 포인트를 꼽을 수 있다.
디버깅을 할 때는 브레이크 포인트를 지정하여 특정 코드가 실행되는 순간에 프로그램을 멈추게 할 수 있다.
프로그램을 중단시키고 디버깅이 끝나면(트랩을 처리하고 나면) **트랩이 발생한 그 다음 명령어**부터 실행해 나간다.
이처럼 예외가 발생한 명령어의 다음 명령어부터 실행을 재개하는 예외를 트랩이라고 한다.

<img src="https://github.com/user-attachments/assets/b1299f30-96e2-4f3e-98df-8a4e550ee0e7" width="420"/><br/>
<br/>
**중단(abort)** 은 CPU가 실행 중인 **프로그램을 강제로 중단**시킬 수밖에 없는 심각한 오류를 발견했을 때 발생하는 예외이다.
그 밖에 **소프트웨어 인터럽트(shoftware interrupt)** 는 시스템 콜이 발생했을 때 발생하는 예외이다.
<br/>
<br/>
### 🍓 CPU 성능 향상을 위한 설계
오늘날 우리가 사용하는 CPU에는 생각했던 것보다 훨씬 더 복잡하고 중요한 개념들이 담겨 있다.
수많은 과학자와 엔지니어들이 조금이라도 더 빠른 CPU를 만들기 위해 새로운 CPU 설계 및 명령어 처리 기법들을 고안해냈기 때문이다.
이번에는 클럭과 코어, 스레드 등 CPU의 성능 향상과 관련된 주요 개념 및 설계 기법, 그리고 이와 관련해 동시성과 병렬성의 정의 및 차이점에 대해 알아보자.

#### [CPU 클럭 속도]
CPU를 구매하려고 하면 판매 페이지마다 빠지지 않고 등장하는 키워드가 있다. 바로 클럭이다. **클럭(clock)** 이란 **컴퓨터의 부품을 일사불란하게 움직일 수 있게 하는 시간의 단위**이다.
클럭의 '똑-딱-똑-딱-' 주기에 맞춰 이 레지스터에서 다른 레지스터로 데이터가 이동하거나 ALU에서 연산이 수행되고, 메모리에 저장된 명령어를 읽어 들이는 것이다.

**클럭 속도**는 헤르츠(Hz) 단위로 측정되는데, 이는 클럭이 1초에 몇 번 반복되는지를 나타낸다. 가령 클럭이 '똑-딱-'하고 1초에 100번 반복되는 CPU가 있다면, 이 CPU의 클럭 속도는 100Hz인 셈이다.
최근에는 CPU의 클럭 속도가 매우 빨라져서 더 빠른 등급인 기가헤르츠(GHz) 단위로 측정하는 것이 일반적이다.

<img src="https://github.com/user-attachments/assets/d8ae8001-736c-41fa-b3b9-b4cf82e5c122" width="450"/><br/>

> 1GHz는 1000000000(10^9)Hz이다.

클럭 속도가 높아지면 CPU는 명령어 사이클을 더 빠르게 반복하고, 다른 부품들도 그에 발맞춰 더 빠르게 작동할 것이라고 기대할 수 있다. 실제로 클럭 속도가 높은 CPU는 일반적으로 성능이 좋다.
이런 점에서 **클럭 속도는 CPU의 속도 단위로 간주되기도 한다.**
하지만 클럭 속도를 필요 이상으로 높이면 컴퓨터의 **발열**이 심해질 수 있기 때문에 클럭 속도를 높이는 것만으로 CPU의 성능을 높이는 데에는 한계가 있다.
<br/>
#### [멀티코어와 멀티스레드]
클럭 속도를 높이는 방법 외에도 코어 수나 스레드 수를 늘리는 방법으로 CPU의 성능을 높일 수 있다. **코어(core)** 란 CPU 내에서 명령어를 읽어 들이고, 해석하고, 실행하는 부품을 의미한다.
앞서 우리는 '명령어를 읽어 들이고, 해석하고, 실행하는 부품'을 CPU라고 배웠다. 이는 많은 전공 서적에서 다루는 전통적인 관점에 따른 정의이다.
전공 서적들의 전통적 관점에서 '명령어를 읽어 들이고, 해석하고, 실행하는 부품'은 원천적으로 CPU 하나만 존재할 수 있었기 때문이다.
하지만 오늘날 기술적 발전을 거듭한 CPU 안에는 '명령어를 읽어 들이고, 해석하고, 실행하는 부품'들 여러 개가 얼마든지 존재할 수 있게 되었고, 이에 코어라는 이름을 붙인 것이다.
<br/>
<br/>
<img src="https://github.com/user-attachments/assets/4faa370f-0080-4178-bca0-8848239ec357" width="600"/><br/>

어떤 CPU를 설명하는 글에 코어가 8개라고 명시되어 있다면 CPU 내부에서 명령어를 읽어 들이고, 해석하고, 실행하는 부품이 8개라는 이야기이다.
이렇게 여러 개의 코어를 포함하고 있는 CPU는 **멀티코어 CPU**, 혹은 **멀티코어 프로세서**라고 부른다. CPU 안에 몇 개의 코어가 포함되어 있는지에 따라 다음과 같이 멀티코어 CPU의 명칭이 바뀌게 된다.

<img src="https://github.com/user-attachments/assets/0484cb78-8817-4c03-9787-976f30d2c36f" width="320"/><br/>
<br/>
이번에는 **스레드**에 대해 알아보자. **스레드(thread)** 의 사전적 의미는 '실행 흐름의 단위'이다. 하지만 이것을 활자 그대로 이해한다면 혼란스러울 수 있다.
'스레드'는 프로그래밍 언어와 CPU, 운영체제에도 등장하는 범용성 높은 용어인 탓에 각각의 용례 또한 조금씩 다르기 때문이다.
혼란을 방지하기 위해 CPU에서 사용하는 **하드웨어적인 스레드(이하 하드웨어 스레드)** 와 프로그래밍 언어 및 운영체제에서 사용하는 **소프트웨어적인 스레드(이하 스레드)** 로 나누어 기억하길 바란다.

**하드웨어 스레드**란 **하나의 코어가 동시에 처리하는 명령어의 단위**를 의미한다.
같은 의미로 '하나의 코어로 여러 명령어를 동시에 처리하는 CPU'를 **멀티스레드(multithread) 프로세서** 혹은 **멀티스레드 CPU**라고 한다.
예를 들어 명령어를 읽어 들이고, 해석하고, 실행하는 부품 하나가 한 번에 하나의 명령어를 처리한다면 이는 **1코어 1스레드 CPU**이고, 명령어를 읽어 들이고, 해석하고, 실행하는 부품 2개가 한 번에
4개의 명령어를 처리한다면 이는 **2코어 4스레드 CPU**인 것이다.

<img src="https://github.com/user-attachments/assets/8969e7ef-a141-4eca-8857-84ab1daebe96" width="320"/><br/>

하드웨어 스레드는 메모리 속 프로그램의 입장에서 봤을 때 '한 번에 하나의 명령어를 처리하는 1개의 CPU'와 다를 바 없다.
마찬가지로 2코어 4스레드 CPU를 메모리 속 프로그램의 입장에서 보면 '한 번에 하나의 명령어를 처리하는 4개의 CPU'가 있는 것처럼 보일 것이다.
그래서 하드웨어 스레드를 **논리 프로세서(logical processor)** 라고 부르기도 한다.

한편, **소프트웨어 스레드**란 **하나의 프로그램에서 독립적으로 실행되는 단위**를 의미한다.
운영체제에 등장하는 스레드가 바로 이 소프트웨어 스레드인데, '어떤 프로그램이 여러 (소프트웨어적)스레드를 통해 실행될 수 있다'는 말은 '메모리에 적재된 해당 프로그램을 구성하는 여러 부분이 동시에
실행될 수 있다'는 말고 같다. 유의할 점은 하드웨어 스레드가 하나인 CPU도 이러한 프로그램을 충분히 실행할 수 있다는 점이다.

직접 확인해보자. 다음은 1코어 1스레드 CPU를 탑재한 컴퓨터 환경(터미널)을 나타낸다. 모든 내용을 깊이 이해할 필요는 없으므로 박스로 표기된 부분만 살펴보겠다.
CPU 코어 수(cpu cores)와 코어별 스레드 수(Thread(s) per core)가 각각 1개인 것을 볼 수 있다.

<img src="https://github.com/user-attachments/assets/11bb7bab-4b03-46dd-9530-ddd9d3806df8" width="600"/><br/>

> 제시된 터미널 결과는 리눅스(우분투) 운영체제를 사용하는 컴퓨터에서 얻었다. 이처럼 리눅스 운영체제에서는 명령어를 통해 컴퓨터의 상태를 알 수 있다.

1코어 1스레드 CPU에서 다음과 같은 소스 코드를 실행했다고 가정해 보자. 코드의 문법은 중요하지 않다.
해당 코드는 3개의 소프트웨어 스레드를 만든 뒤, 'task1, task2, task3'라는 함수를 각 스레드로 실행하는 파이썬 코드이다.
코드를 실행하면 스레도 'thread1, thread2, thread3'가 생성되어 각자 'task1, task2, task3'를 동시에 실행할 것이다.

#### [arch/thread.py]
```python
import threading
import time

def task1():
    for _ in range(5):
        print("Task 1 is running")
        time.sleep(1)

def task2():
    for _ in range(5):
        print("Tast 2 is running")
        time.sleep(1)

def task3():
    for _ in range(5):
        print("Tast 3 is running")
        time.sleep(1)

thread1 - threading.Thread(target=task1)
thread2 - threading.Thread(target=task2)
thread3 - threading.Thread(target=task3)

thread1.start()
thread2.start()
thread3.start()

thread1.join()
thread2.join()
thread3.join()

print("All tasks are done.")
```
코드의 실행 결과는 어떻게 될까? 1코어 1스레드 CPU를 사용 중이므로 실행이 불가능할까? 그렇지 않다. 코드는 다음과 같이 정상적으로 잘 작동한다.

#### [실행 결과]
```
Task 1 is running
Task 2 is running
Task 3 is running
Task 1 is running
Task 2 is running
Task 3 is running
Task 1 is running
Task 2 is running
Task 3 is running
Task 1 is running
Task 2 is running
Task 3 is running
Task 1 is running
Task 2 is running
Task 3 is running
All tasks are done.
```
앞서 설명했던 스레드의 사전적 정의만을 암기했다면 '1코어 1스레드 CPU가 여러 스레드로 만들어진 프로그램을 실행할 수 있다'는 말을 납득하기 어려울 것이다.
따라서 하드웨어적인 스레드와 소프트웨어적인 스레드의 의미를 구분하여 기억해 두는 것이 좋다.

<img src="https://github.com/user-attachments/assets/eeefc504-c047-4442-b0f5-0b6a54f70507" width="450"/><br/>
<br/>
지금까지 알아본 CPU 클럭 속도와 멀티코어, 멀티스레드는 윈도우 운영체제의 [작업 관리자] 창의 [성능] 탭에서 CPU의 '속도, 코어, 논리 프로세서' 항목으로 확인할 수 있다.

<img src="https://github.com/user-attachments/assets/1b30ce5a-5f40-482c-8fdf-65826e77adea" width="470"/><br/>
<br/>

하드웨어 스레드와 소프트웨어 스레드의 차이는 동시성과 병렬성이라는 키워드의 차이를 통해 좀 더 명확히 이해할 수 있다.
동시성과 병렬성은 모두 '여러 작업이 동시에 처리되는 양상'을 표현하는 단어지만, 세부적인 의미는 다르다.

**병렬성(parallelism)** 은 작업을 물리적으로 동시에 처리하는 성질이다. 하드웨어 스레드가 4개인 CPU가 4개의 명령어를 동시에 실행하는 경우를 생각해 보자.
같은 시점에 여러 작업을 동시에 처리할 수 있을 것이다. 이것이 바로 병렬성의 예이다.

반면, **동시성(concurrency)** 은 동시에 작업을 처리하는 것처럼 보이는 성질을 의미한다.
이 CPU가 빠르게 작업을 번갈아 가며 처리할 경우, 사용자의 눈에는 마치 여러 작업이 동시에 처리되는 것처럼 보일 수 있지만, 물리적으로 같은 시점에 여러 작업이 동시에 처리되고 있는 것은 아니다.
이것이 동시성의 에시이다.

<img src="https://github.com/user-attachments/assets/f1b5405a-f9a5-44b5-87be-4d506084221c" width="470"/><br/>
<br/>
즉, 하드웨어 스레드는 '병렬성'을 구현하기 위한 물리적인 실행 단위에 가깝고, 소프트웨어 스레드는 '동시성'을 구현하기 위한 논리적인 실행 단위에 가깝다.
<br/>
<br/>
### 🍓 파이프라이닝을 통한 명령어 병렬 처리
**명령어 병렬 처리 기법(ILP, Instruction-Level Parallelism)** 은 여러 명령어를 동시에 처리하여 CPU를 한시도 쉬지 않고 작동시킴으로써 CPU의 성능을 높이는 기법을 말한다.
현대 CPU의 명령어 처리에서 절대 빠질 수 없는 핵심 기술이라고 할 수 있다. 다양한 종류가 있지만, 꼭 기억해야 할 명령어 병렬 처리 기법은 **명령어 파이프라이닝**이다.

명령어 파이프라이닝을 이해하려면 우선 하나의 명령어가 처리되는 과정을 비슷한 시간 간격으로 나누어 보아야 한다. 일반적으로 다음과 같은 간격으로 나눌 수 있다.

- [1] 명령어 인출(Instruction Fetch)
- [2] 명령어 해석(Instruction Decode)
- [3] 명령어 실행(Execute Instruction)
- [4] 결과 저장(Write Back)

여기서 중요한 점은 같은 단계가 겹치지만 않는다면 CPU가 **각각의 단계를 동시에 실행할 수 있다**는 점이다.
CPU는 하나의 명령어가 **인출**되는 동안 다른 명령어를 **실행**할 수 있고, 하나의 명령어가 실행되는 동안 연산의 결과를 **저장**할 수 있다. 이를 그림으로 표현하면 다음과 같다.
t1에서는 명령어 1과 2를 동시에 처리할 수 있고, t2에서는 명령어 1, 2, 3을 동시에 처리할 수 있다.
이처럼 공장의 생산 라인과 같이 명령어들을 **명령어 파이프라인(instruction pipeline)** 에 넣고 동시에 처리하는 기법을 **명령어 파이프라이닝(instrucctino pipelining)** 이라고 한다.

<img src="https://github.com/user-attachments/assets/644a228f-0a32-4635-8b4e-2098e17d6205" width="430"/><br/>
<br/>

> #### 슈퍼스칼라
> 오늘날 대부분의 CPU는 여러 개의 파이프라인을 이용한다. 이처럼 **CPU 내부에 여러 명령어 파이프라인을 포함하는 구조를 슈퍼스칼라(superscalar)** 라고 한다.
> 명령어 파이프라인을 하나만 두는 것이 마치 공장의 생산 라인을 하나만 둔 것과 같다면, 슈퍼스칼라는 공장의 생산 라인을 여러 개 두는 것과 같다.
> 슈퍼스칼라 구조로 명령어 처리가 가능한 CPU는 **슈퍼스칼라 프로세서**, 혹은 **슈퍼스칼라 CPU**라고 부른다.
>
> <img src="https://github.com/user-attachments/assets/76fab62e-8a3a-4c11-a4c5-cf3839f8adaa" width="420"/><br/>

이때 CPU가 이해하고 실행하는 명령어 집합 중 명령어 파이프라이닝을 비롯한 명령어 병렬 처리에 유리한 명령어 집합이 있고, 불리한 명령어 집합이 있다는 점에 유의해야 한다.
앞서 명령어의 종류와 생김새가 CPU마다 다를 수 있다고 강조한 바 있는데, 파이프라이닝 성능의 차이를 보이는 대표적인 명령어 집합 유형으로 CISC와 RISC를 꼽을 수 있다.
대표적인 CISC 기반 CPU로는 인텔의 x86 혹은 x86-64 CPU가 있고, RISC 기반 CPU로는 애플의 M1 CPU가 있다. 어셈블리어 예시(p63)를 통해 CISC 명령어와 RISC 명령어의 생김새를 살펴볼 수 있ㄷ다.

**CISC(Complex Instruction Set Computer)** 는 이름 그대로 다채로운 기능을 지원하는 복잡한 명령어들로 구성된 명령어 집합이다. 그렇기 때문에 적은 수의 명령어로도 프로그램을 실행할 수 있다.
반대로, **RISC(Reduced Instruction Set Computer)** 는 CISC에 비해 활용 가능한 명령어의 종류가 적다. CISC와는 달리 짧고 규격화된 명령어, 되도록이면 1클럭 내외로 실행되는 명령어를 지향한다.
같은 프로그램이라 하더라도 RISC에는 CISC보다 많은 명령어가 필요하다.

여기까지 살펴보면 언뜻 RISC보다 CISC가 더 우월한 명령어 집합이라고 생각할 수 있지만, CISC는 활용하는 명령어가 워낙 복잡하고 다양한 기능을 제공하는 탓에 명령어의 크기 및 실행되기까지의 시간이
일정하지 않고, 하나의 명령어 실행에 여러 클럭 주기가 필요하다. 그래서 CISC는 RISC에 비해 명령어의 수행 시간이 길고 들쑥날쑥하기 때문에 파이프라이닝에 비효율적일 수 있다.
규격화되지 않은 명령어가 파이프라이닝을 어렵게 만든 셈이다. 반면, RISC는 CISC에 비해 크기가 규격화되어 있고, 하나의 명령어가 1클럭 내외로 실행되기 때문에 파이프라이닝에 최적화되어 있다.

<img src="https://github.com/user-attachments/assets/f20addd9-3f8e-4856-9a9e-f558bc8e6f58" width="480"/><br/>
<br/>

마지막으로 파이프라이닝이 CPU 성능 향상에 실패하는 경우도 알아보자.
파이프라이닝이 실패하여 성능 향상이 이루어지지 않는 상황은 **파이프라인 위험(pipeline hazard)** 이라고 부르며, **데이터 위험**과 **제어 위험, 구조적 위험**으로 구분할 수 있다.
**데이터 위험(data hazard)** 은 명령어 간의 **데이터 의존성**에 의해 발생한다. 어떤 명령어들은 동시에 처리할 수 없고, 이전 명령어를 끝까지 실행해야만 비로소 실행할 수 있다.
예를 들어 다음의 두 명령어 중 명령어 2는 명령어 1에 의존적이다. 명령어 1의 저장까지 끝낸 뒤에야 비로소 명령어 2를 인출할 수 있기 때문이다.
이렇게 의존성이 있는 두 명령어를 무작정 겹쳐서 실행하면 파이프라인이 제대로 작동하지 않을 수 있으며, 이를 데이터 위험이라고 한다.

<img src="https://github.com/user-attachments/assets/f91a5e8e-0d69-4085-9c0c-55b3f1829fbf" width="600"/><br/>
<br/>
> 이해를 돕기 위해 편의상 레지스터의 이름을 R1, R2, R3, R4, R5, '왼쪽 레지스터에 오른쪽 레지스터 내용을 저장하라'는 명령을 ← 기호로 표기했다.

**제어 위험(control hazard)** 은 **프로그램 카운터의 갑작스러운 변화**에 의해 발생한다. 프로그램 카운터는 기본적으로 1씩 증가하며, '현재 실행 중인 명령어의 다음 주소'로 갱신된다.
하지만 JUMP, CONDITIONAL JUMP, 인터럽트 등으로 인해 프로그램 실행의 흐름이 바뀌어 명령어가 실행되면서 프로그램 카운터 값에 갑작스러운 변화가 생기면 미리 인출하거나 해석 중인 명령어들은
아무 쓸모가 없어지게 된다. 이를 제어 위험이라고 한다.

**구조적 위험(structural hazard)** 은 명령어들을 겹쳐 실행하는 과정에서 **서로 다른 명령어가 동시에 ALU, 레지스터 등 같은 CPU 부품을 사용**하려고 할 때 발생한다.
구조적 위험은 **자원 위험(resource hazard)** 이라고도 부른다.