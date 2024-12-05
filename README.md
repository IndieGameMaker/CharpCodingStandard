# C# 코딩 표준안

- Unity 프로젝트에서 사용할 스타일 가이드 예제입니다.
- 팀의 선호도에 따라 규칙을 추가하거나 생략하세요.

```csharp

// - Microsoft의 프레임워크 디자인 가이드:
//   https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/

// - Google의 스타일 가이드:
//   https://google.github.io/styleguide/csharp-style.html

// - 이러한 규칙을 조정하여 팀의 스타일 가이드를 구축하고 일관되게 적용하세요.
// - 확신이 서지 않을 때는 팀의 스타일 가이드를 따르세요.

// 이름/대소문자 표기법:
// - Pascal 표기법(예: ExamplePlayerController, MaxHealth 등)을 사용하세요. 별도 명시되지 않는 한.
// - 로컬/프라이빗 변수나 매개변수에는 camel 표기법(예: examplePlayerController, maxHealth 등)을 사용하세요.
// - snake 표기법, kebab 표기법, 헝가리안 표기법은 피하세요.
// - MonoBehaviour 클래스는 소스 파일 이름과 일치해야 합니다.

// 포맷팅:
// - K&R(여는 중괄호를 같은 줄에 배치) 또는 Allman(여는 중괄호를 새로운 줄에 배치) 스타일을 선택하세요.
// - 한 줄을 짧게 유지하세요. 가로 여백을 고려하고, 표준 줄 길이(80-120자)를 정하세요.
// - 흐름 제어 조건 앞에 한 칸의 공백을 추가하세요(예: while (x == y)).
// - 대괄호 내부에는 공백을 사용하지 마세요(예: x = dataArray[index]).
// - 함수 인수 사이에 쉼표 다음 한 칸의 공백을 추가하세요.
// - 함수 인수 뒤에 공백을 추가하지 마세요(예: CollectItem(myObject, 0, 1)).
// - 함수 이름과 괄호 사이에 공백을 넣지 마세요(예: DropPowerUp(myPrefab, 0, 1)).
// - 시각적 구분을 위해 빈 줄을 추가하세요.

// 주석:
// - "무엇"이나 "어떻게"만 답하지 말고 "왜"를 설명하세요.
// - `//` 주석을 사용하여 논리와 가까이 설명을 추가하세요.
// - 직렬화된 필드에는 주석 대신 Tooltip을 사용하세요.
// - #region은 피하세요. 큰 클래스 크기를 권장하고 접힌 코드는 읽기 어렵습니다.
// - 법적 정보나 라이선싱은 외부 참조 링크로 처리하여 공간을 절약하세요.
// - 공용 메서드나 함수 앞에는 XML 주석을 사용하여 출력 문서/Intellisense에 도움을 주세요.

// USING 구문:
// - 파일 상단에 using 구문을 유지하세요.
// - 사용하지 않는 using 구문은 제거하세요.

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
```

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

// 네임스페이스:
// - Pascal 표기법 사용. 특수 기호나 밑줄을 사용하지 마세요.
// - 네임스페이스를 반복하지 않도록 파일 상단에 using 구문을 추가하세요.
// - 하위 네임스페이스를 점(.) 연산자를 사용하여 생성하세요(예: MyApplication.GameFlow, MyApplication.AI 등).
namespace StyleSheetExample
{
    // 열거형:
    // - 단수형 타입 이름을 사용하세요.
    // - 접두사나 접미사를 사용하지 마세요.
    public enum Direction
    {
        North, // 북
        South, // 남
        East,  // 동
        West,  // 서
    }

    // 플래그 열거형:
    // - 복수형 타입 이름을 사용하세요.
    // - 접두사나 접미사를 사용하지 마세요.
    // - 2진수 값에는 열을 정렬하세요.
    [Flags]
    public enum AttackModes
    {
        // 십진수                           // 2진수
        None = 0,                          // 000000
        Melee = 1,                         // 000001
        Ranged = 2,                        // 000010
        Special = 4,                       // 000100

        MeleeAndSpecial = Melee | Special  // 000101
    }

    // 인터페이스:
    // - 인터페이스 이름은 형용사 구문으로 작성하세요.
    // - 'I' 접두사를 사용하세요.
    public interface IDamageable
    {
        string damageTypeName { get; } // 데미지 타입 이름
        float damageValue { get; }    // 데미지 값

        // 메서드:
        // - 메서드 이름은 동사 또는 동사 구문으로 시작하여 동작을 나타내세요.
        // - 매개변수 이름은 camelCase를 사용하세요.
        bool ApplyDamage(string description, float damage, int numberOfHits); // 데미지를 적용하는 메서드
    }

    public interface IDamageable<T>
    {
        void Damage(T damageTaken); // 데미지를 처리하는 메서드
    }

    // 클래스 또는 구조체:
    // - 이름은 명사 또는 명사 구문으로 작성하세요.
    // - 접두사는 사용하지 마세요.
    // - 파일에 MonoBehaviour 클래스가 하나만 있어야 하며, 파일 이름은 클래스와 일치해야 합니다.
    public class StyleExample : MonoBehaviour
    {
        // 필드:
        // - 특수 문자는 사용하지 마세요(백슬래시, 기호, 유니코드 문자). 이는 명령줄 도구와 충돌할 수 있습니다.
        // - 이름은 명사로 작성하되, boolean은 동사로 시작하세요.
        // - 의미 있는 이름을 사용하세요. 검색 가능하고 발음 가능한 이름으로 작성하세요. 약어는 사용하지 마세요(수학 관련 제외).
        // - 공용 필드에는 PascalCase를, private 변수에는 camelCase를 사용하세요.
        // - 로컬 변수와 구분하기 위해 private 필드에는 밑줄(_)을 추가하세요.
        // - 더 명확한 접두사(m_ = 멤버 변수, s_ = static, k_ = const)를 사용할 수도 있습니다.
        // - 기본 접근 제한자를 명시하거나 생략하되, 스타일 가이드에 일관성을 유지하세요.

        private int _elapsedTimeInDays; // 경과된 일 수

        // [SerializeField] 속성을 사용하면 private 필드도 Inspector에 표시됩니다.
        [SerializeField] private bool _isPlayerDead; // 플레이어가 죽었는지 여부

        // PlayerStats 클래스에서 데이터를 그룹화하여 Inspector에 표시합니다.
        [SerializeField] private PlayerStats _stats;

        // 값의 범위를 제한하고 Inspector에 슬라이더를 생성합니다.
        [Range(0f, 1f)] [SerializeField] private float _rangedStat;

        // Tooltip은 주석을 대체할 수 있으며 추가 정보를 제공합니다.
        [Tooltip("플레이어의 또 다른 통계입니다.")]
        [SerializeField] private float _anotherStat;

        // 프로퍼티:
        // - 공용 필드 대신 사용하는 것을 선호하세요.
        // - PascalCase를 사용하며 특수 문자는 사용하지 마세요.
        // - 표현식 본문 프로퍼티를 사용하여 짧게 작성하되, 선호하는 형식을 선택하세요.
        // - 예: 읽기 전용에는 표현식 본문 프로퍼티를 사용하고, 그 외에는 { get; set; } 사용.
        // - 백업 필드가 필요 없는 경우 자동 구현 프로퍼티를 사용하세요.

        private int _maxHealth; // private 백업 필드

        public int MaxHealthReadOnly => _maxHealth; // 읽기 전용 프로퍼티

        public int MaxHealth
        {
            get => _maxHealth; // getter
            set => _maxHealth = value; // setter
        }

        public int Health { private get; set; } // 쓰기 전용

        public void SetMaxHealth(int newMaxValue) => _maxHealth = newMaxValue; // setter

        public string DescriptionName { get; set; } = "Fireball"; // 자동 구현 프로퍼티

        // 이벤트:
        // - 동사 구문으로 이름을 작성하세요.
        // - 현재분사(present participle)는 "이전" 상태, 과거분사(past participle)는 "이후" 상태를 나타냅니다.
        public event Action OpeningDoor; // 문 열기 이벤트 (전)
        public event Action DoorOpened;  // 문 열기 이벤트 (후)

        public event Action<int> PointsScored; // 점수 획득 이벤트
        public event Action<CustomEventArgs> ThingHappened; // 커스텀 이벤트

        public void OnDoorOpened()
        {
            DoorOpened?.Invoke(); // 이벤트 호출
        }

        public void OnPointsScored(int points)
        {
            PointsScored?.Invoke(points); // 점수 이벤트 호출
        }

        // 커스텀 이벤트 인자 구조체
        public struct CustomEventArgs
        {
            public int ObjectID { get; } // 객체 ID
            public Color Color { get; } // 색상

            public CustomEventArgs(int objectId, Color color)
            {
                ObjectID = objectId;
                Color = color;
            }
        }

        // 메서드:
        // - 동사나 동사 구문으로 시작하여 동작을 나타내세요.
        // - 매개변수 이름은 camelCase를 사용하세요.
        public void SetInitialPosition(float x, float y, float z)
        {
            transform.position = new Vector3(x, y, z);
        }

        public bool IsNewPosition(Vector3 newPosition)
        {
            return (transform.position == newPosition); // 위치 비교
        }

        private void FormatExamples(int someExpression)
        {
            // VAR:
            // - 가독성을 높이는 경우 var를 사용하세요.
            // - 타입이 모호해지는 경우 var 사용을 피하세요.
            var powerUps = new List<PlayerStats>();
            var dict = new Dictionary<string, List<GameObject>>();

            // SWITCH 문:
            // - 형식을 선택하여 일관성을 유지하세요.
            switch (someExpression)
            {
                case 0:
                    // ..
                    break;
                case 1:
                    // ..
                    break;
                case 2:
                    // ..
                    break;
            }

            // 중괄호:
            // - 단일 줄 문에서도 명확성을 위해 중괄호를 유지하세요.
            for (int i = 0; i < 100; i++) { DoSomething(i); }

            for (int i = 0; i < 100; i++)
            {
                DoSomething(i);
            }

            for (int i = 0; i < 10; i++)
            {
                for (int j = 0; j < 10; j++)
                {
                    DoSomething(j);
                }
            }
        }

        private void DoSomething(int x)
        {
            // ...
        }
    }

    // 기타 클래스:
    // - 필요한 만큼 다른 헬퍼/MonoBehaviour가 아닌 클래스를 정의하세요.
    // - 이 구조체는 Inspector에서 필드를 그룹화합니다.
    [Serializable]
    public struct PlayerStats
    {
        public int MovementSpeed; // 이동 속도
        public int HitPoints; // 히트 포인트
        public bool HasHealthPotion; // 체력 회복 포션 소지 여부
    }
}

```
