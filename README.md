# 20210803
Unit 48. 구조체 사용하기
1) 구조체: 변수 여러 개를 체계적으로 관리하기 위한 자료구조. 
    struct 로 정의 (data structure의 약자_자료구조)
    
      #구조체 정의
      struct 구조체이름 {
          자료형 멤버 이름;
      };
      
      #구조체를 변수로 선언
      struct 구조체이름 변수이름;
      
        #define _CRT_SECURE_NO_WARNINGS
        #include <stdio.h>
        #include <string.h>
      
        struct Person {       // 구조체 정의
          char name[20];    // 구조체 멤버
          int age;
          char address[100];
        };
      
        int main()
        {
          sturct Person p1;  // 구조체 변수 선언
          
          // 점으로 구조체 멤버에 접근하여 값 할당
          strcpy(p1.name, "홍길동");
          p1.age = 30;
          strspy(p1.address, "서울시 용산구 한남동");
          
          // 점으로 구조체 멤버에 접근하여 값 출력
          printf("이름: %d\n", p1.name);
          printf("나이: %d\n", p1.age);
          printf("주소: %d\n", p1.address);
          
          return 0;
        }
      

2)구조체를 선언하는 동시에 변수를 선언하기

      struct Person{
          char name[20];
          int age;
          char address[100];
      } p1;
      
      int main()
      {
          strcpy(p1.name, "홍길동");
          p1.age = 30;
          strcpy(p1.address, "서울시 용산구 한남동");
          
          return 0;
      }
      
  - p1은 전역변수

3)구조체 변수를 선언하는 동시에 초기화하기
  - struct 구조체이름 변수이름 = {.멤버 = 값, 멤버 = 값};
  - struct 구조체이름 변수이름 = {값, 값} // 이 때 순서 중요! 빈 칸 있으면 안 됨!

        struct Person {
            char name[20];
            int age;
            char address[100];
        };
        
        int main()
        {
            struct Person p1 = { .name = "홍길동", .age = 30, .address = "서울시 용산구 한남동")
            struct Person p2 = { "고길동", 40, "서울시 서초구 반포동"}
        
            return 0;
        }
        
          
4)typedef로 struct 키워드없이 구조체 선언하기

typedef struct 구조체 이름{
    자료형 멤버이름;
} 구조체별칭;

int main()
{
    구조체별칭 변수이름;
}

      typedef struct _Person {   // 구조체 정의
          char name[20];
          int age;
          char address[100];
      } Person;         // 구조체 별칭

      int main()
      {
          Person p1; // 구조체 별칭으로 변수 선언
          
          strcpy(p1.name, "홍길동");
          p1.age = 30;
          strcpy(p1.address, "서울시 용산구 한남동");
          
          return 0;
      }
      
- struct _Person p1; 과 
  Person p1;은 완전히 값음. 
  
  
  
  
5) typedef : 자료형의 별칭을 만드는 기능
- typedef로 정의한 별칭: 사용자 정의 자료형, 사용자 정의 타입

typedef 자료형 별칭
typedef 자료형* 별칭

      typedef int MYINT;   // int를 별칭 MYINT로 정의
      typedef int* PMYINTㅣ // int포인터를 별칭 PMYUNT로 정의
      
      MYINT num1;   // MYINT로 변수 선언
      PMYINT numPtr1; // PMYINT로 포인터 변수 선언
      
      numPtr1 = &num1; // 포인터에 변수의 주소 저장
      PMYINT *numPtr1;   // 이중포인터
      
      
6) 익명 구조체: 구조체 이름이 없는 구조체
- struct 뒤에 나오는 구조체 이름을 '태그(tag)'라고 함. 관례상 앞에 _를 붙임.
typedef로 구조체를 정의하면서 구조체이름을 생략할 수 있음.

      typedef struct {    // 구조체 이름이 없는 익명 구조체
          char name[20];
          int age;
          char address[100];
      }; Person    // 구조체 별칭
      
    
Unit 49. 구조체 포인터 사용하기
1) 구조체 변수를 일일이 선언해서 사용하는 것보다는, 포인터에 메모리를 할당해서 사용하는 편이 효율적임.

    int main()
    {
        struct Person p1 = malloc(sizeof(Person));
        
        // 화살표 연산자로 구조체 멤버에 접근하여 값 할당
        strcpy(p1->names, "홍길동");    
        p1->age = 30;   // 이는 (*p1).age = 30;과 같음.
        
        return 0;
    }
    
    <주의>
    - p1.name을 역참조-> *p1.name : 멤버의 역참조이지, 구조체변수의 역참조가 아님.
    -                 -> *p1->name : 얘도 멤버의 역참조. 
        
            struct Data {
                char c1;
                int *numPtr;
            };
            
            int main()
            {
                int num = 10;
                struct Data d1;   // 구조체 변수 선언
                struct Data *d2 = malloc(sizeof(struct Data));   // 구조체 포인터에 메모리 할당
                
                d1.numPtr = &num1;
                d2->numptr = &num1;
                
                printf("%d\n", *d1.numPtr);   // 구조체 멤버를 역참조
                printf("%d\n", d2->numPtr);   // 구조체 포인터의 멤버를 역참조
                
                d2->c1 = 'a';   
                printf("%c\n", (*d2).c1);   // a, 구조체 포인터를 역참조하여 c1에 접근. d2->c1과 같은 의미임. 
                                            // (*d2): d2가 가리키고 있는 주소이니, struct Data. pointer to struct Data에서 pointer to 가 제거되서 struct Data를 의미하게 됨.  
                printf("%d\n", *(*d2).numPtr);   // 구조체 포인터를 역참조해서 numPtr에 접근한 뒤 다시 역참조.
                                                // *d2->numPtr과 같음.
                                                
                free(d2);
                
                return 0;
                
            }
                
            
    
Unit 51. 구조체 멤버 정렬 사용하기
0)구조체

      #include <stdio.h>
     
      sturct PacketHeader { 
          char flags; // 1바이트
          int seql;   // 4바이트
      };

컴퓨터에서 CPU가 메모리에 접근할 때, 32비트CPU는 4바이트 / 64비트CPU는 8바이트 단위로 접근합니다. 
만약 32비트 CPU에서 4비트보다 작은 단위로 데이터에 접근하게 되면 (안에서 시프트 연산이 발생) 효율이 떨어짐.
그래서 C언어 컴파일러는, CPU가 메모리의 데이터에 효과적으로 접근할 수 있도록 '구조체'를 만들 때 일정한 크기로 정렬함. 

1) 구조체의 크기 알아보기


