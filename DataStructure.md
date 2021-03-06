# Data Structure 

## Recursion
> During executing algorithm or function, technique solving problem by recalling itself
* Example
  ```c
  #include <stdio.h>

  int fibonacci(int num)
  {
      if (num<=0)
          return 0;
      else if (num == 1)
          return 1;
      return fibonacci(num - 2) + fibonacci(num - 1);
  }

  int main()
  {
      int input = 0;
      int i = 0;

      printf("입력: ");
      scanf("%d", &input);

      for ( i = 0; i < input; i++)
      {
          printf("%d ", fibonacci(i));
      }
      puts("");
  }
  
  ```

## Linear Data Structure
### List
#### Linear list (Array)
> Coincide logical, physical order. (Data are sequentially saved without empty space when data insert, delete.)
* <strong>Search => O(n)</strong>
* <strong>Traversal => O(n)</strong>
* <strong>Insert => (last : O(1), middle : O(n))</strong>
* <strong>Delete => O(n)</strong>
<br/>

* <strong>ArrayList.h</strong>

  ```c

  #ifndef __ARRAY_LIST_H__
  #define __ARRAY_LIST_H__

  #define TRUE 1
  #define FALSE 0
  #define LIST_LEN 100

  //ArrayList의 정의
  typedef int dataType;

  typedef struct __ArrayList
  {
    dataType arr[LIST_LEN];	 
    int numberOfData;	     
  } ArrayList; 


  //ArrayList calculation
  typedef ArrayList List; 
  void listInit(List* plist); //init
  void appendList(List* plist, dataType data); //insert
  void removeList(List* plist, dataType data); //delete
  dataType returnList(List* plist, int index); // return element
  void sortList(List* plist, int start, int end); // sort
  int searchList(List* plist, dataType data);  // search


  #endif

  ```

* <strong>main.c</strong>

  ```c
  #include<stdio.h>
  #include "ArrayList.h"

  int main()
  {
    List list; //create
    listInit(&list);//init
    appendList(&list, 20); //insert elements
    appendList(&list, 40);
    appendList(&list, 30);
    appendList(&list, 20);
    appendList(&list, 23);
    appendList(&list, 12);
    appendList(&list, 13);
    appendList(&list, 10);
    printf("현재의 데이터 수는 %d\n", list.numberOfData);
    for (int i = 0; i < list.numberOfData; i++)
    {
      printf("%d ", returnList(&list, i));
    }
    printf("\n");

    removeList(&list, 30); // delete a element

    printf("현재의 데이터 수는 %d\n", list.numberOfData);
    for (int i = 0; i < list.numberOfData; i++)
    {
      printf("%d ", returnList(&list, i));
    }
  }
  ```



#### Linked list (Pointer)
> Not Coincide logical, physical order. (Even if logical order is changed by insert and delete, change only link informtion and not change physcial order.)
* <strong>Search => O(n)</strong>
* <strong>Traversal => O(n)</strong>
* <strong>Insert => O(n)</strong>
* <strong>Delete => O(n)</strong>

<strong>1. Singly Linked List</strong>
  * A node has structure linked next node by a link field
    ![image](https://user-images.githubusercontent.com/64727012/172037978-e2ee555b-f297-44e8-9d4f-cb7d8dafe62d.png)

  * Example
  ```c
  #include <stdio.h>
  #include <stdlib.h>
  #include <string.h>

  // 단순 연결 리스트의 노드 구조를 구조체로 정의
  typedef struct ListNodeType {
    char data[2];
    struct ListNodeType* link;
  } ListNode;

  // 리스트의 시작을 나타내는 header 노드를 구조체로 정의
  typedef struct LinkedListType{
    ListNode* headerNode;
  } LinkedList;

  // 공백 연결 리스트를 생성하는 연산
  LinkedList* createLinkedList(void) {
    LinkedList* pList;
    pList = (LinkedList*)malloc(sizeof(LinkedList));
    pList->headerNode = NULL;		// 공백 리스트이므로 NULL로 설정
    return pList;
  }

  // 연결 리스트의 전체 메모리를 해제하는 연산
  void freeLinkedList(LinkedList* pList) {
    ListNode* pNode;
    while (pList->headerNode != NULL) {
      pNode = pList->headerNode;
      pList->headerNode = pList->headerNode->link;
      free(pNode);
      pNode = NULL;
    }
  }

  // 연결 리스트를 순서대로 출력하는 연산
  void displayList(LinkedList* pList) {
    ListNode* pNode;
    printf("pList = (");
    pNode = pList->headerNode;
    while (pNode != NULL) {
      printf("%s", pNode->data);
      pNode = pNode->link;
      if (pNode != NULL) printf(", ");
    }
    printf(") \n");
  }

  // 맨 앞 노드로 삽입하는 연산
  void insertFrontNode(LinkedList* pList, char* a) {
    ListNode* newNode;
    newNode = (ListNode*)malloc(sizeof(ListNode));	// 삽입할 새 노드 할당
    strcpy(newNode->data, a);						// 새 노드의 데이터 필드에 a 저장  
    newNode->link = pList->headerNode;
    pList->headerNode = newNode;
  }

  // 노드를 pre 뒤에 삽입하는 연산
  void insertMiddleNode(LinkedList* pList, ListNode* pre, char* a) {
    ListNode* newNode;
    newNode = (ListNode*)malloc(sizeof(ListNode));
    strcpy(newNode->data, a);
    if (pList == NULL) {				// 공백 리스트인 경우
      newNode->link = NULL;		// 새 노드를 첫 번째이자 마지막 노드로 연결
      pList->headerNode = newNode;
    }
    else if (pre == NULL) {			// 삽입 위치를 지정하는 포인터 pre가 NULL인 경우
      pList->headerNode = newNode;			// 새 노드를 첫 번째 노드로 삽입
    }
    else {
      newNode->link = pre->link;	// 포인터 pre의 노드 뒤에 새 노드 연결
      pre->link = newNode;
    }
  }

  // 마지막 노드로 삽입하는 연산 
  void insertEndNode(LinkedList* pList, char* a) {
    ListNode* newNode;
    ListNode* temp;
    newNode = (ListNode*)malloc(sizeof(ListNode));
    strcpy(newNode->data, a);
    newNode->link = NULL;
    if (pList->headerNode == NULL) {			// 현재 리스트가 공백인 경우					
      pList->headerNode = newNode;			// 새 노드를 리스트의 시작 노드로 연결
      return;
    }
    // 현재 리스트가 공백이 아닌 경우 	
    temp = pList->headerNode;
    while (temp->link != NULL) temp = temp->link;	// 현재 리스트의 마지막 노드를 찾음
    temp->link = newNode;							// 새 노드를 마지막 노드(temp)의 다음 노드로 연결 
  }

  // 리스트에서 노드 pNode를 삭제하는 연산
  void removeNode(LinkedList* pList, ListNode* pNode) {
    ListNode* pre;					// 삭제할 노드의 선행자 노드를 나타낼 포인터	
    if (pList->headerNode == NULL) return;		// 공백 리스트라면 삭제 연산 중단	
    if (pList->headerNode->link == NULL) {    // 리스트에 노드가 한 개만 있는 경우
      free(pList->headerNode);				// 첫 번째 노드를 메모리에서 해제하고
      pList->headerNode = NULL;				// 리스트 시작 포인터를 NULL로 설정
      return;
    }
    else if (pNode == NULL) return;		// 삭제할 노드가 없다면 삭제 연산 중단	
    else {							// 삭제할 노드 pList의 선행자 노드를 포인터 pre를 이용해 찾음
      pre = pList->headerNode;
      while (pre->link != pNode) {
        pre = pre->link;
      }
      pre->link = pNode->link;			// 삭제할 노드 pNode의 선행자 노드와 다음 노드를 연결
      free(pNode);					// 삭제 노드의 메모리 해제	 		
    }
  }

  // 리스트에서 a 값을 가지고 있는 노드를 탐색하는 연산
  ListNode* searchNode(LinkedList* pList, char* a) {
    ListNode *temp;
    temp = pList->headerNode;
    while (temp != NULL) {
      if (strcmp(temp->data, a) == 0) return temp;
      else temp = temp->link;
    }
    return temp;
  }


  int main() {
    LinkedList* pList;
    ListNode* pNode;
    pList = createLinkedList();		// 공백 리스트 생성

    printf("(1) 리스트에 [료], [조] 노드 삽입하기! \n");
    insertFrontNode(pList, "료"); 
    insertEndNode(pList, "조"); 
    displayList(pList); 
    getchar();

    printf("(2) 리스트에서 [료] 노드 탐색하기! \n");
    pNode = searchNode(pList, "료");
    if (pNode == NULL) printf("찾는 데이터가 없습니다.\n");
    else printf("pNode = [%s] 찾았습니다.\n", pNode->data);
    getchar();

    printf("(3) 리스트의 [료] 노드 뒤에 [의]노드 삽입하기! \n");
    insertMiddleNode(pList, pNode,  "의"); 
    displayList(pList); 
    getchar();

    printf("(4) 리스트에 [자] 노드를 노드의 첫번째에 삽입하기! \n");
    insertFrontNode(pList, "자");
    displayList(pList); 
    getchar();

    printf("(5) 리스트에서 [의] 노드 탐색하기! \n");
    pNode = searchNode(pList, "의");
    if (pNode == NULL) printf("찾는 데이터가 없습니다.\n");
    else printf("pNode = [%s] 찾았습니다.\n", pNode->data);
    getchar();

    printf("(6) 리스트의 [의] 노드 뒤에 [구] 노드 삽입하기! \n");
    insertMiddleNode(pList, pNode, "구");
    displayList(pList); 
    getchar();

    printf("(7) 리스트에서 [의] 노드 삭제하기! \n");
    pNode = searchNode(pList, "의");		// 삭제할 노드 위치 pNode를 찾음
    removeNode(pList, pNode);			// 포인터 pNode 노드 삭제
    displayList(pList); 
    getchar();

    printf("(8) 리스트에서 [의] 노드 탐색하기! \n");
    pNode = searchNode(pList, "의");
    if (pNode == NULL) printf("찾는 데이터가 없습니다.\n");
    else printf("pNode = [%s] 찾았습니다.\n", pNode->data);
    getchar();

    printf("(9) 리스트의 메모리 해제! \n");
    freeLinkedList(pList);		// 리스트의 메모리 해제
    displayList(pList); 
    getchar();

    return 0;
  }

  ```

<strong>3. Circular Linked List</strong>
  * same with Singly Linked List except for part linking a last node to a first node 
    ![image](https://user-images.githubusercontent.com/64727012/172038139-f53e4372-3ce1-4a9e-995c-dd146150867b.png)
  * Example
    ```c
    
    #include <stdio.h>
    //원형연결리스트 정의
    typedef char dataType;
    //리스트 노드구조
    typedef struct CircularLinkedNodeType {
      dataType data;
      struct CircularLinkedNodeType* tail;
    } CLinkedNode;

    //원형연결리스트 생성(front) head->tail이 첫 인덱스
    typedef struct CircularLinkedList {
      CLinkedNode* head;
      int nodeCount;
    }CLinkedList;

    //원형연결리스트 생성
    CLinkedList* createCLinkedList()
    {
      CLinkedList* pList = (CLinkedList*)malloc(sizeof(CLinkedList));
      pList->head = (CLinkedNode*)malloc(sizeof(CLinkedNode));;
      pList->nodeCount = 0;
      return pList;
    }
    //원형연결리스트 노드삽입(변수 세개)(공백리스트, 번호, 데이터)
    void insertCLinkedList(CLinkedList* plist, unsigned int index, dataType data)
    {
      CLinkedNode* preNode = plist->head->tail;
      CLinkedNode* newNode = (CLinkedNode*)malloc(sizeof(CLinkedNode));

      newNode->data = data;//데이터 삽입
      if (plist->nodeCount == 0) {
        newNode->tail = newNode;
        plist->head->tail = newNode;
        plist->nodeCount++;
        return 0;
      }

      if (index == 0) {
        for (int i = 0; i < plist->nodeCount - 1; i++) {
          preNode = preNode->tail;
        }
        preNode->tail = newNode;
        newNode->tail = plist->head->tail;
        plist->head->tail = newNode;
      }
      else {
        for (int i = 0; i < index - 1; i++) {
          preNode = preNode->tail;
        }
        newNode->tail = preNode->tail;
        preNode->tail = newNode;
      }
      plist->nodeCount++;
    }
    //원형연결리스트 노드삭제
    void removeCLinkedList(CLinkedList* plist, unsigned int index) {

      CLinkedNode* pre = plist->head->tail;
      CLinkedNode* temp;
      for (int i = 0; i < index - 1; i++) {
        pre = pre->tail;
      }
      temp = pre->tail;
      pre->tail = pre->tail->tail;
      free(temp);

      plist->nodeCount--;
    }
    //원형연결리스트 노드탐색(원소값찾기)
    dataType returnNodeData(CLinkedList* plist, int index) {

      CLinkedNode* temp = plist->head->tail;

      for (int i = 0; i < index; i++) {
        temp = temp->tail;
      }
      return temp->data;
    }
    //원형연결리스트 노드개수반환
    int returnNodeCount(CLinkedList* plist) {
      return plist->nodeCount;
    }
    //원형연결리스트 전체원소반환
    void display(CLinkedList* plist) {
      int i;
      CLinkedNode* node = (CLinkedNode*)malloc(sizeof(CLinkedNode));
      node = plist->head->tail;
      printf("( ");
      for (i = 0; i < plist->nodeCount; i++) {
        printf("%c ", node->data);
        node = node->tail;
      }
      printf(")\n");
    }
    int main()
    {
      //원형 리스트 생성
      CLinkedList *list = createCLinkedList();
      //원형 리스트 삽입
      insertCLinkedList(list, 0, 'S');
      insertCLinkedList(list, 0, 'F');
      insertCLinkedList(list, 0, 'U');
      insertCLinkedList(list, 0, 'H');
      insertCLinkedList(list, 4, '/');
      insertCLinkedList(list, 5, 'S');
      insertCLinkedList(list, 5, 'E');
      insertCLinkedList(list, 5, 'C');

      //리스트 노드 갯수 반환
      printf("현재 리스트의 갯수는 %d입니다.\n", returnNodeCount(list));
      //전체 리스트 노드 원소 반환
      display(list);
      //노드탐색 인덱스원소반환
      printf("%d번 인덱스의 문자는 %c입니다.\n", 6, returnNodeData(list, 6));
      //노드삭제 인덱스노드삭제
      removeCLinkedList(list, 3);
      display(list);
      printf("현재 리스트의 갯수는 %d입니다.\n", returnNodeCount(list));
      removeCLinkedList(list, 4);
      display(list);
      printf("현재 리스트의 갯수는 %d입니다.\n", returnNodeCount(list));
    }

    
    ```


<strong>4. Doubly Linked List</strong>
  * Improved Linked List adding a link for pointing a prior node
    * Doubly Linked List
    
      ![image](https://user-images.githubusercontent.com/64727012/172038334-f4dc4d8e-d485-49fc-a0b6-ab6aadb796fb.png)
    * Circular Doubly Linked List
    
      ![image](https://user-images.githubusercontent.com/64727012/172038347-b0bb2773-9e65-4dd7-997d-3dc77b414610.png)

  * Example
    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>

    // 이중 연결 리스트의 노드 구조를 구조체로 정의
    typedef struct ListNodeType {
      struct ListNodeType* llink;   // 왼쪽(선행) 노드에 대한 링크
      char data[2];
      struct ListNodeType* rlink;   // 오른쪽(다음) 노드에 대한 링크
    } DListNode;

    // 리스트 시작을 나타내는 HeaderNode 노드를 구조체로 정의
    typedef struct {
      DListNode* headerNode;
    } DLinkedList;


    // 공백 이중 연결 리스트를 생성하는 연산
    DLinkedList* createDLinkedList(void) {
      DLinkedList* pDList;
      pDList = (DLinkedList*)malloc(sizeof(DLinkedList));	// 헤드 노드 할당
      pDList->headerNode = NULL;					// 공백 리스트이므로 NULL로 설정
      return pDList;
    }

    // 연결 리스트의 전체 메모리를 해제하는 연산
    void freeLinkedList(DLinkedList* pDList) {
      DListNode* pNode;
      while (pDList->headerNode != NULL) {
        pNode = pDList->headerNode;
        pDList->headerNode = pDList->headerNode->rlink;
        free(pNode);
        pNode = NULL;
      }
    }

    // 이중 연결 리스트를 순서대로 출력하는 연산
    void displayList(DLinkedList* pDList) {
      DListNode* pNode;
      printf(" pDList = (");
      pNode = pDList->headerNode;
      while (pNode != NULL) {
        printf("%s", pNode->data);
        pNode = pNode->rlink;
        if (pNode != NULL) printf(", ");
      }
      printf(") \n");
    }
    // 맨 앞 노드로 삽입하는 연산
    void insertFrontNode(DLinkedList* pDList, char* a) {
      DListNode* newNode;
      newNode = (DListNode*)malloc(sizeof(DListNode));	// 삽입할 새 노드 할당
      strcpy(newNode->data, a);						// 새 노드의 데이터 필드에 a 저장  

      if (pDList== NULL)         // 공백 리스트인 경우
        newNode->rlink = NULL;
      else
          newNode->rlink = pDList->headerNode;
      newNode->llink = NULL;
      pDList->headerNode = newNode;
    }

    // pre 뒤에 노드를 삽입하는 연산
    void insertMiddleNode(DLinkedList* pDList, DListNode* pre, char* a) {
      DListNode* newNode;
      newNode = (DListNode*)malloc(sizeof(DListNode));
      strcpy(newNode->data, a);

      if ((pDList== NULL)|| (pre == NULL)) {         // 공백 리스트 or pre가 NULL인 경우
        newNode->rlink = NULL;
        newNode->llink = NULL;
        pDList->headerNode = newNode;   // 새 노드를 첫 번째이자 마지막 노드로 연결
      }
      else {
        newNode->rlink = pre->rlink;
        pre->rlink = newNode;
        newNode->llink = pre;
        if (newNode->rlink != NULL)	// 삽입 자리에 다음 노드가 있는 경우
          newNode->rlink->llink = newNode;
      }
    }
    // 마지막 노드로 삽입하는 연산 
    void insertEndNode(DLinkedList* pDList, char* a) {
      DListNode* newNode;
      DListNode* temp;
      newNode = (DListNode*)malloc(sizeof(DListNode));
      strcpy(newNode->data, a);

      if (pDList->headerNode == NULL) {			// 현재 리스트가 공백인 경우			
        newNode->rlink = NULL;
        newNode->llink = NULL;	
        pDList->headerNode = newNode;			// 새 노드를 리스트의 시작 노드로 연결
        return;
      }
      // 현재 리스트가 공백이 아닌 경우 	
      temp = pDList->headerNode;
      while (temp->rlink != NULL) temp = temp->rlink;	// 현재 리스트의 마지막 노드를 찾음
      newNode->llink = temp;
      temp->rlink = newNode;							// 새 노드를 마지막 노드(temp)의 다음 노드로 연결 
      newNode->rlink = NULL;
    }

    // 이중 연결 리스트에서 del 노드를 삭제하는 연산
    void removeNode(DLinkedList* pDList, DListNode* del) {
      if (pDList->headerNode == NULL) return;	// 공백 리스트인 경우, 삭제 연산 중단		
      else if (del == NULL) return;	// 삭제할 노드가 없는 경우, 삭제 연산 중단	
      else {
        del->llink->rlink = del->rlink;
        del->rlink->llink = del->llink;
        free(del);					// 삭제 노드의 메모리 해제	 		
      }
    }

    // 리스트에서 a 노드를 탐색하는 연산
    DListNode* searchNode(DLinkedList* pDList, char* a) {
      DListNode *temp;
      temp = pDList->headerNode;
      while (temp != NULL) {
        if (strcmp(temp->data, a) == 0) return temp;
        else temp = temp->rlink;
      }
      return temp;
    }

    int main() {
      DLinkedList* pDList;
      DListNode *pNode;
      pDList = createDLinkedList();  // 공백 리스트 생성

      printf("(1) 이중 연결 리스트 생성하기! \n");
      displayList(pDList); 
      getchar();

      printf("(2) 이중 연결 리스트의 첫번째에 [료] 노드 삽입하기! \n");
      insertFrontNode(pDList, "료");
      displayList(pDList); 
      getchar();

      printf("(3) 이중 연결 리스트의 [료] 노드 뒤에 [의] 노드 삽입하기! \n");
      pNode = searchNode(pDList, "료"); 
      insertMiddleNode(pDList, pNode, "의");
      displayList(pDList); 
      getchar();

      printf("(4) 이중 연결 리스트의 마지막에 [조] 노드 삽입하기! \n");
      insertEndNode(pDList, "조");
      displayList(pDList); 
      getchar();

      printf("(5) 이중 연결 리스트의 첫번째에 [자] 노드 삽입하기! \n");
      insertFrontNode(pDList, "자");
      displayList(pDList); 
      getchar();

      printf("(6) 리스트에서 [의] 노드 탐색하기! \n");
      pNode = searchNode(pDList, "의");
      if (pNode == NULL) printf("찾는 데이터가 없습니다.\n");
      else printf("pNode = [%s] 찾았습니다.\n", pNode->data);
      getchar();

      printf("(7) 이중 연결 리스트의 [의] 노드 뒤에 [구] 노드 삽입하기! \n");
      insertMiddleNode(pDList, pNode, "구");
      displayList(pDList); 
      getchar();

      printf("(8) 이중 연결 리스트에서 [의] 노드 삭제하기! \n");
      pNode = searchNode(pDList, "의");      	
      removeNode(pDList, pNode);
      displayList(pDList); 
      getchar();

      printf("(9) 리스트에서 [의] 노드 탐색하기! \n");
      pNode = searchNode(pDList, "의");
      if (pNode == NULL) printf("찾는 데이터가 없습니다.\n");
      else printf("pNode = [%s] 찾았습니다.\n", pNode->data);
      getchar();

      printf("(10) 리스트의 메모리 해제! \n");
      freeLinkedList(pDList);		// 리스트의 메모리 해제
      displayList(pDList); 
      getchar();
      return 0;
    }
    
    ```

### stack
* <strong>Accessible only at top position
* Last-In-First-Out (LIFO) 
  * Insert/Delete : only at top postition => O(1)
  * Peek => O(1)</strong>
  
  ![image](https://user-images.githubusercontent.com/64727012/172039199-c91b3a75-1b54-4559-ad3e-9d6522a31be2.png)

* Example (Infix)
  ```c
  #include<stdio.h>
  #include<string.h>
  #include<stdlib.h>

  typedef struct StackNode {
    char data;
    struct StackNode* front;
  } Node;

  typedef struct ArrayStack {
    Node* top;
    int index;
  } Stack;

  Stack* init() {
    Stack* pstack = (Stack*)malloc(sizeof(Stack));
    pstack->top = (Node*)malloc(sizeof(Node));
    pstack->index = -1;
    return pstack;
  }

  void push(Stack* pstack, int a) {
    Node* node = (Node*)malloc(sizeof(Node));
    node->front = NULL;
    node->data = a;
    if (pstack->index == -1) {
      pstack->top->front = node;
    }
    else {
      node->front = pstack->top->front;
      pstack->top->front = node;
    }
    pstack->index++;
  }

  void pop(Stack* pstack) {
    Node* temp;
    temp = pstack->top->front;
    pstack->top->front = pstack->top->front->front;
    free(temp);
    pstack->index--;
  }

  void infixToPostfix(char* exp) {
    Stack* operator = init();
    Stack* sen = init();
    int i = 0, count = 0;

    while (exp[i] != '\0') {
      if (exp[i] == '+' || exp[i] == '-' || exp[i] == '*' || exp[i] == '/') {
        if (exp[i] == '+' || exp[i] == '-') {
          push(operator, exp[i]);
        }
        else {
          push(sen, exp[i + 1]);
          push(sen, exp[i]);
          i += 2;
          count += 2;
          continue;
        }
      }
      else if (exp[i] == ')') {
        push(sen, operator->top->front->data);
        pop(operator);
        count++;
      }
      else if (exp[i] == '(') {
        i++;
        continue;
      }
      else {
        push(sen, exp[i]);
        count++;
      }
      i++;
    }
    if (operator->index != -1) {
      for (int i = 0; i <= operator->index; i++) {
        push(sen, operator->top->front->data);
        pop(operator);
        count++;
      }
    }

    char* temp = (char*)malloc(sizeof(char) * count);
    for (int i = 0; i < count; i++) {
      temp[count - 1 - i] = sen->top->front->data;
      pop(sen);
    }
    for (int i = 0; i < count; i++)
      exp[i] = temp[i];
    for (int i = count; i < strlen(exp); i++) {
      exp[i] = '\0';
    }
  }

  int evalPostfix(char *exp) {
    int result = 0;
    int i = 0;
    int temp1;
    int temp2;
    int sum;
    int tmp;
    Stack* num = init();
    char c;

    while (exp[i] != '\0') {
      if (exp[i] == '+') {
        temp1 = num->top->front->data;
        pop(num);
        temp2 = num->top->front->data;
        pop(num);
        sum = temp2 + temp1;
        push(num, sum);
      }
      else if (exp[i] == '-') {
        temp1 = num->top->front->data;
        pop(num);
        temp2 = num->top->front->data;
        pop(num);
        sum = temp2 - temp1;
        push(num, sum);
      }
      else if (exp[i] == '/') {
        temp1 = num->top->front->data;
        pop(num);
        temp2 = num->top->front->data;
        pop(num);
        sum = temp2 / temp1;
        push(num, sum);
      }
      else if (exp[i] == '*') {
        temp1 = num->top->front->data;
        pop(num);
        temp2 = num->top->front->data;
        pop(num);
        sum = temp1 * temp2;
        push(num, sum);
      }
      else {
        c = exp[i];
        push(num, atoi(&c));
      }
      i++;
    }
    return num->top->front->data;
  }


  int main(void) {
    int result;
    char express[] = "((2-5)+(3*4-1)-(9/3))";	// 중위 연산식


    printf("중위 표기식 : %s\n", express);

    infixToPostfix(express);                            // 후위 표기법
    printf("후위 표기식 : %s", express);

    result = evalPostfix(express);                    // 후위 연산식 계산
    printf("\n\n연산 결과 => %d", result);

    getchar();
  }

  
  ```

### Queue
* <strong>A first-In element is deleted first
* First-In-First-out (FIFO)
  * Insert/Delete => O(1)</strong>

  ![image](https://user-images.githubusercontent.com/64727012/172039374-76223ce2-b179-4d19-b838-0418f9d8c6d5.png)
* Example
  ```c
  #include <stdio.h>
  #include <stdlib.h>
  #define MAX_QUEUE_SIZE	200 

  typedef struct TaskCustomer {
    int	id;
    int	tArrival;
    int	tService;
  } Customer;

  typedef Customer Element;

  typedef struct CircularQueue {
    Element data[MAX_QUEUE_SIZE];	// 요소의 배열
    int	front;			// 전단
    int	rear;			// 후단
  } Queue;

  void error(char* str)
  {
    fprintf(stderr, "%s\n", str);
    exit(1);
  };
  // 큐의 주요 연산: 공통
  void init_queue(Queue *q) { q->front = q->rear = 0; ; }
  int is_empty(Queue *q) { return q->front == q->rear; }
  int is_full(Queue *q) { return q->front == (q->rear + 1) % MAX_QUEUE_SIZE; }
  int size(Queue *q) { return(q->rear - q->front + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE; }

  void enqueue(Queue *q, Element val)
  {
    if (is_full(q))
      error("  큐 포화 에러");
    q->rear = (q->rear + 1) % MAX_QUEUE_SIZE;
    q->data[q->rear] = val;
  }
  Element dequeue(Queue *q)
  {
    if (is_empty(q))
      error("  큐 공백 에러");
    q->front = (q->front + 1) % MAX_QUEUE_SIZE;
    return q->data[q->front];
  }
  Element peek(Queue *q)
  {
    if (is_empty(q))
      error("  큐 공백 에러");
    return q->data[(q->front + 1) % MAX_QUEUE_SIZE];
  }

  int	nSimulation;		// 시뮬레이션 시간 
  double probArrival;		// 단위시간에 도착하는 평균 고객 수 
  int	tMaxService;		// 한 고객에 대한 최대 서비스 시간 
  int	totalWaitTime;		// 전체 대기 시간 
  int	nCustomers;		    // 전체 고객의 수 
  int	nServedCustomers;	// 서비스를 받은 전체 고객 수 

  double rand0to1() { return rand() / (double)RAND_MAX; }

  void insert_customer(Queue *q, int arrivalTime)
  {
    Customer a;

    a.id = ++nCustomers;
    a.tArrival = arrivalTime;
    a.tService = (int)(tMaxService*rand0to1()) + 1;
    printf("  고객 %d 방문 (서비스 시간:%d분)\n", a.id, a.tService);
    enqueue(q, a);
  }
  void read_sim_params()
  {
    printf("시뮬레이션 할 최대 시간 (예:20) = ");
    scanf("%d", &nSimulation);
    printf("단위시간에 도착하는 고객 수 (예:0.5) = ");
    scanf("%lf", &probArrival);
    printf("한 고객에 대한 최대 서비스 시간 (예:~6) = ");
    scanf("%d", &tMaxService);
    printf("====================================================\n");
  }
  void run_simulation()
  {
    Queue q;
    Customer a;

    int clock = 0;
    int serviceTime = -1;

    init_queue(&q);
    nCustomers = totalWaitTime = nServedCustomers = 0;
    while (clock < nSimulation) {
      clock++;
      printf("현재시각=%d\n", clock);

      if (rand0to1() > probArrival)
        insert_customer(&q, clock);
      if (serviceTime>0) serviceTime--;
      else {
        if (is_empty(&q)) {
          printf("  현재 대기 고객 수       = %d\n", nCustomers - nServedCustomers);
          continue;
        }
        a = dequeue(&q);
        nServedCustomers++;
        totalWaitTime += clock - a.tArrival;
        printf("  고객 %d 서비스 시작 (대기시간:%d분)\n", a.id, clock - a.tArrival);
        serviceTime = a.tService - 1;
      }
      printf("  현재 대기 고객 수       = %d\n", nCustomers - nServedCustomers);
    }
  }

  void print_result()
  {
    printf("=================================================\n");
    printf("  서비스 받은 고객수      = %d\n", nServedCustomers);
    printf("  전체 대기 시간          = %d분\n", totalWaitTime);
    printf("  서비스고객 평균대기시간 = %-5.2f분\n",
      (double)totalWaitTime / nServedCustomers);
    printf("  현재 대기 고객 수       = %d\n", nCustomers - nServedCustomers);

  }
  #include <time.h>
  void main()
  {
    srand((unsigned int)time(NULL));
    read_sim_params();
    run_simulation();
    print_result();
  }
  ```

#### Deque (Double-Ended Queue)
* Expanded queue capable of insertion and deletion at both ends.

  ![image](https://user-images.githubusercontent.com/64727012/172039685-b590b1f1-74ed-4d6e-bbdd-ee9cf36eb063.png)
* Example
  ```c
  #include<stdio.h>

  typedef char Datatype;

  typedef struct QueueNode {
      Datatype data;
      struct QueueNode* next;
  }Node;

  typedef struct ArrayQueue {
      int index;
      Node* rear;
      Node* front;
  }Queue;

  Queue* init() {
      Queue* temp = (Queue*)malloc(sizeof(Queue));
      temp->front = (Node*)malloc(sizeof(Node));
      temp->rear = (Node*)malloc(sizeof(Node));
      temp->index = -1;
      return temp;
  }
  int isEmpty(Queue* pQueue) {
      if (pQueue->index == -1) {
          return 1;
      }
      else return 0;
  }

  void qInsertRear(Queue* pQueue, char data) {
      Node* temp = (Node*)malloc(sizeof(Node));
      Node* tmp = pQueue->front->next;
      temp->data = data;
      temp->next = NULL;

      if (isEmpty(pQueue) == 1) {
          pQueue->front->next = temp;
          pQueue->rear->next = temp;
          temp->next = temp;
      }
      else {
          pQueue->rear->next->next = temp;
          temp->next = pQueue->front->next;
          pQueue->rear->next = temp;
      }
      pQueue->index++;
  }

  void qInsertFront(Queue* pQueue, char data) {
      Node* temp = (Node*)malloc(sizeof(Node));

      temp->data = data;
      temp->next = NULL;

      if (isEmpty(pQueue) == 1) {
          pQueue->front->next = temp;
          pQueue->rear->next = temp;
          temp->next = temp;
      }
      else {
          temp->next = pQueue->front->next;
          pQueue->rear->next->next = temp;
          pQueue->front->next = temp;
      }
      pQueue->index++;
  }

  void qDeleteFront(Queue* pQueue) {
      if (isEmpty(pQueue) == 1)return 0;
      else {
          Node* tempF = (Node*)malloc(sizeof(Node));
          tempF = pQueue->front->next;
          pQueue->rear->next->next = pQueue->front->next->next;
          free(tempF);
          pQueue->front->next = pQueue->rear->next->next;
      }
      pQueue->index--;
  }

  void qDeleteRear(Queue* pQueue) {

      if (isEmpty(pQueue) == 1)return 0;
      else {
          Node* tempR = (Node*)malloc(sizeof(Node));
          Node* tmp = (Node*)malloc(sizeof(Node));
          tmp = pQueue->rear->next;
          tempR = pQueue->front->next;

          for (int i = 0; i < pQueue->index - 1; i++) {
              tempR = tempR->next;
          }
          tempR->next = pQueue->front->next;
          pQueue->rear->next = tempR;
          free(tmp);
      }
      pQueue->index--;
  }
  Datatype getQueueFront(Queue* pQueue) {
      return pQueue->front->next->data;
  }
  Datatype getQueueRear(Queue* pQueue) {
      return pQueue->rear->next->data;
  }
  void Palindrome(char* str) {
      printf("%s\n", str);
      int i = 0;
      Queue* sen = init();
      while (str[i] != '\0') {
          if (str[i] == ' ') {
              i++;
              continue;
          }
          else {
              qInsertRear(sen, str[i]);
              i++;
          }
      }

      int count = sen->index;

      if ((sen->index + 1) % 2 == 1) {
          for (int i = 0; i < count / 2; i++) {
              if (getQueueFront(sen) == getQueueRear(sen)) {
                  qDeleteFront(sen);
                  qDeleteRear(sen);

              }
              else {
                  printf("회문이아닙니다.\n\n");
                  return 0;
              }
          }
          printf("회문입니다.\n\n");
          return 0;
      }
      else {
          for (int i = 0; i < (count+1) / 2; i++) {
              if (getQueueFront(sen) == getQueueRear(sen)) {
                  qDeleteFront(sen);
                  qDeleteRear(sen);
              }
              else {
                  printf("회문이아닙니다.\n\n");
                  return 0;
              }
          }
          printf("회문입니다.\n\n");
          return 0;
      }
  }

  int main() {

      char str0[] = "madondam";
      char str1[] = "radar";
      char str2[] = "rotavator";
      char str3[] = "madam";
      char str4[] = "was it an cat i saw";
      char str5[] = "a man a plan a canal panama";
      char str6[] = "race car";
      char str7[] = "was it a cat i saw";
      char str8[] = "nurses run";
      char str9[] = "a man a plan an canal panama";

      // Palindrome checker
      Palindrome(str0);
      Palindrome(str1);
      Palindrome(str2);
      Palindrome(str3);
      Palindrome(str4);
      Palindrome(str5);
      Palindrome(str6);
      Palindrome(str7);
      Palindrome(str8);
      Palindrome(str9);

      return 0;
  }
  ```

## Non-Linear Data Structure

### Tree
* <strong>Non-linear data structure having 1:n relation between elements
* Hierarchical Data Structure</strong>

#### Term
* <strong>Position in tree
  * Root node => A first node of tree
  * Leaf node => nodes not having child nodes
  * Internal node => nodes having child nodes
* Relation between nodes
  * Parent node => Parent and child node are connected by edge
  * Child node
  * Ancestor node => All nodes in route from root node to parent node
  * descendent node => All nodes below a specific node
  * Sibling node => Child nodes of same parent node
* Property of node
  * Level => Distance from root node
  * Height => Value that plus 1 at height of a child node that is at the longest distance from a root node 
  * Degree => number of child node that a node has </strong> 

#### Complete Binary Tree
* <strong>All levels except last level are filled completely
* Even if last level is not filled fully, a node must be filled from left to right
* Implementation way : Array, Linked Data structure
* Example : Preorder, Inorder, Postorder</strong>
  ```c
  #include<stdio.h>

  typedef char datatype;

  typedef struct BinaryTree {
    datatype data;
    struct BinaryTree* lLink;
    struct BinaryTree* rLink;
  }Tree;

  Tree* init() {
    Tree* temp = (Tree*)malloc(sizeof(Tree));
    temp->lLink = NULL;
    temp->rLink = NULL;
  }

  void setData(Tree* pTree, datatype data) {
    pTree->data = data;
  }

  void makeLeftTree(Tree* sub, Tree* main) {
    main->lLink = sub;
  }
  void makeRightTree(Tree* sub, Tree* main) {
    main->rLink = sub;
  }


  void preorder(Tree* pTree) {
    if (pTree == NULL) return 0;
    printf("%c ", pTree->data);
    preorder(pTree->lLink);
    preorder(pTree->rLink);
  }
  void inorder(Tree* pTree) {
    if (pTree == NULL) return 0;
    inorder(pTree->lLink);
    printf("%c ", pTree->data);
    inorder(pTree->rLink);
  }
  void postorder(Tree* pTree) {
    if (pTree == NULL) return 0;
    postorder(pTree->lLink);
    postorder(pTree->rLink);
    printf("%c ", pTree->data);
  }

  int wholeNodeCount(Tree* pTree) {
    if (pTree == NULL) return 0;
    int left = wholeNodeCount(pTree->lLink);
    int right = wholeNodeCount(pTree->rLink);
    return 1 + left + right;
  }

  int terminalNodeCount(Tree* pTree) {
    if (pTree == NULL) return 0;
    if (pTree->lLink == NULL && pTree->rLink == NULL) return 1;
    int left = terminalNodeCount(pTree->lLink);
    int right = terminalNodeCount(pTree->rLink);
    return left + right;
  }
  int rootNodeHeight(Tree* pTree) {
    if (pTree == NULL) return 0;
    int left = rootNodeHeight(pTree->lLink);
    int right = rootNodeHeight(pTree->rLink);
    if (left > right) {
      return 1 + left;
    }
    else
      return 1 + right;
  }

  int main()
  {
    Tree* head = init();
    Tree* t1 = init();
    Tree* t2 = init();
    Tree* t3 = init();
    Tree* t4 = init();
    Tree* t5 = init();
    Tree* t6 = init();
    Tree* t7 = init();
    Tree* t8 = init();
    Tree* t9 = init();
    Tree* t10 = init();

    setData(head, 'A');
    setData(t1, 'B');
    setData(t2, 'C');
    setData(t3, 'D');
    setData(t4, 'E');
    setData(t5, 'F');
    setData(t6, 'G');
    setData(t7, 'H');
    setData(t8, 'I');
    setData(t9, 'J');
    setData(t10, 'K');

    makeLeftTree(t1, head);
    makeRightTree(t2, head);
    makeLeftTree(t3, t1);
    makeRightTree(t4, t1);
    makeLeftTree(t5, t2);
    makeRightTree(t6, t2);
    makeLeftTree(t7, t3);
    makeLeftTree(t8, t4);
    makeRightTree(t9, t4);
    makeRightTree(t10, t6);

    printf("preorder : ");
    preorder(head);
    printf("\n\ninorder : ");
    inorder(head);
    printf("\n\npostorder : ");
    postorder(head);
    printf("\n\n이진 트리 노드 개수 : %d", wholeNodeCount(head));
    printf("\n\n이진 트리 단말 노드 개수 : %d", terminalNodeCount(head));
    printf("\n\n이진 트리 높이 : %d", rootNodeHeight(head));
  }
  
  ```

### Priority queue and Heap
* <strong>Priority queue => queue saving data that have priority</strong>
  * Not FIFO, but data that have high priority are first out
  * Important calculation => Insert/Delete
  * Unordered array => Insertion = O(1) / Deletion = O(n)
  * Unordered linked list => Insertion = O(1) / Deletion = O(n)
  * Sorted array => Insertion = O(n) / Deletion = O(1)
  * Sorted linked list => Insertion = O(n) / Deletion = O(1)
  * <strong>Heap => Insertion(upHeap) = O(log n) / Deletion(downHeap) = O(log n)</strong>

* <strong>Heap
  * Complete Binary Tree
  * Max heap => Complete Binary Tree that key value of parent node is greater than or equal to child node
    * Get max-value => O(1)
  * Min heap => Complete Binary Tree that key value of parent node is less than or equal to child node
    * Get min-value => O(1)
  * Example : Huffman code</strong>
    ```c
    
    #include <stdio.h>
    #include <stdlib.h>
    #include <memory.h>

    typedef struct treeNode {	// 연결 자료구조로 구성하기 위해 트리의 노드 정의
      char data;
      struct treeNode* left;  // 왼쪽 서브 트리에 대한 링크 필드
      struct treeNode* right; // 오른쪽 서브 트리에 대한 링크 필드
    } treeNode;

    // data를 루트 노드로 하여 왼쪽 서브 트리와 오른쪽 서브 트리를 연결하는 연산
    treeNode* makeRootNode(char data, treeNode* leftNode, treeNode* rightNode) {
      treeNode* root = (treeNode*)malloc(sizeof(treeNode));
      root->data = data;
      root->left = leftNode;
      root->right = rightNode;
      return root;
    }
    void decryption(treeNode* root, int zipData[]) {
      int i = 0;
      treeNode* temp = (treeNode*)malloc(sizeof(treeNode));
      temp = root;

      printf("복호화 : ");

      while (1) {
        if (zipData[i] == 0) {
          if (temp->left == NULL) {
            printf("%c", temp->data);
            temp = root;
            continue;
          }
          temp = temp->left;
        }
        else {
          if (temp->right == NULL) {
            printf("%c", temp->data);
            temp = root;
            continue;
          }
          temp = temp->right;
        }
        i++;
        if (zipData[i] != 1 && zipData[i] != 0) {
          printf("%c", temp->data);
          break;
        }
      }
    }

    void main() {
      // 허프만 코드 이진 트리 만들기
      treeNode* n11 = makeRootNode('u', NULL, NULL);
      treeNode* n10 = makeRootNode('i', NULL, NULL);
      treeNode* n9 = makeRootNode('h', NULL, NULL);
      treeNode* n8 = makeRootNode('5', n10, n11);
      treeNode* n7 = makeRootNode('f', NULL, NULL);
      treeNode* n6 = makeRootNode('s', NULL, NULL);
      treeNode* n5 = makeRootNode('4', n8, n9);
      treeNode* n4 = makeRootNode('n', NULL, NULL);
      treeNode* n3 = makeRootNode('3', n6, n7);
      treeNode* n2 = makeRootNode('2', n4, n5);
      treeNode* n1 = makeRootNode('1', n2, n3);

      // 허프만 zip data
      int huffman_zip[] = { 0,1,1,0,1,0,1,1,1,1,0,0,1,0,0,1,0,1,1,0,1,0,1,0,0 };
      // 복호화
      decryption(n1, huffman_zip);
    }
    
    ```

### Graph
* <strong>Data structure representing n:n relation between linked elements</strong>
  * e.g. subway, bus route, etc.  

#### Graph Definition
* G, graph, is represented by (V,E).
  * Collection of vertex and edge. 
* V (vertices) 
  * Objects capable of having various characteristic
  * V(G) => Collection of vertices of graph G.  
* E (edge)
  * Relation between vertices
  * E(G) => Collection of edges of graph G. 

![image](https://user-images.githubusercontent.com/64727012/172055281-90df9f7e-8d5f-4958-9295-d6a5dd575b4a.png)

#### Graph Implementation
<strong>1. Adjacency Matrix</strong>
  
  ![image](https://user-images.githubusercontent.com/64727012/172078442-ea6a4ace-2c39-42b6-b639-68e38b40d6e7.png)


  * Example
    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #define MAX_VERTEX 30

    // 그래프를 인접 행렬로 표현하기 위한 구조체 정의
    typedef struct graphType {
      int n;									// 그래프의 정점 개수
      int adjMatrix[MAX_VERTEX][MAX_VERTEX];  // 그래프에 대한 30x30의 2차원 배열
    } graphType;

    // 공백 그래프를 생성하는 연산
    void createGraph(graphType* g) {
      int i, j;
      g->n = 0;								// 정점 개수를 0으로 초기화
      for (i = 0; i<MAX_VERTEX; i++) {
        for (j = 0; j<MAX_VERTEX; j++)
          g->adjMatrix[i][j] = 0;			// 그래프 g에 대한 2차원 배열의 값을 0으로 초기화
      }
    }

    // 그래프 g에 정점 v를 삽입하는 연산
    void insertVertex(graphType* g, int v) {
      if (((g->n) + 1)>MAX_VERTEX) {
        printf("\n 그래프 정점의 개수를 초과하였습니다!");
        return;
      }
      g->n++; // 그래프 정점의 개수 n을 하나 증가
    }

    // 그래프 g에 간선 (u, v)를 삽입하는 연산
    void insertEdge(graphType* g, int u, int v) {
      // 간선 (u, v)를 삽입하기 위해 정점 u와 v가 그래프에 존재하는지 확인
      if (u >= g->n || v >= g->n) {
        printf("\n 그래프에 없는 정점입니다!");
        return;
      }
      g->adjMatrix[u][v] = 1;// 삽입한 간선에 대한 2차원 배열의 원소 값을 1로 설정
    }

    // 그래프 g의 2차원 배열 값을 순서대로 출력하는 연산
    void print_adjMatrix(graphType* g) {
      int i, j;
      for (i = 0; i<(g->n); i++) {
        printf("\n\t\t");
        for (j = 0; j<(g->n); j++)
          printf("%2d", g->adjMatrix[i][j]);
      }
    }

    void main() {
      int i;
      graphType *G1, *G2, *G3, *G4;
      G1 = (graphType *)malloc(sizeof(graphType));
      G2 = (graphType *)malloc(sizeof(graphType));
      G3 = (graphType *)malloc(sizeof(graphType));
      G4 = (graphType *)malloc(sizeof(graphType));
      createGraph(G1); createGraph(G2); createGraph(G3); createGraph(G4);

      // 그래프 G1
      for (i = 0; i<4; i++)
        insertVertex(G1, i);  // G1의 정점 0~3 삽입 
      insertEdge(G1, 0, 1);
      insertEdge(G1, 0, 3);
      insertEdge(G1, 1, 0);
      insertEdge(G1, 1, 2);
      insertEdge(G1, 1, 3);
      insertEdge(G1, 2, 1);
      insertEdge(G1, 2, 3);
      insertEdge(G1, 3, 0);
      insertEdge(G1, 3, 1);
      insertEdge(G1, 3, 2);
      printf("\n G1의 인접 행렬");
      print_adjMatrix(G1);

      // 그래프 G2
      for (i = 0; i<3; i++)
        insertVertex(G2, i);  // G2의 정점 0~2 삽입 
      insertEdge(G2, 0, 1);
      insertEdge(G2, 0, 2);
      insertEdge(G2, 1, 0);
      insertEdge(G2, 1, 2);
      insertEdge(G2, 2, 0);
      insertEdge(G2, 2, 1);
      printf("\n\n G2의 인접 행렬");
      print_adjMatrix(G2);

      // 그래프 G3
      for (i = 0; i<4; i++)
        insertVertex(G3, i); // G3의 정점 0~3 삽입
      insertEdge(G3, 0, 1);
      insertEdge(G3, 0, 3);
      insertEdge(G3, 1, 2);
      insertEdge(G3, 1, 3);
      insertEdge(G3, 2, 3);
      printf("\n\n G3의 인접 행렬");
      print_adjMatrix(G3);

      // 그래프 G4
      for (i = 0; i<3; i++)
        insertVertex(G4, i);    // G4의 정점 0~2 삽입 
      insertEdge(G4, 0, 1);
      insertEdge(G4, 0, 2);
      insertEdge(G4, 1, 0);
      insertEdge(G4, 1, 2);
      printf("\n\n G4의 인접 행렬");
      print_adjMatrix(G4);

      getchar();
    }
    ```

<strong>2. Adjacency List</strong>
  
  ![image](https://user-images.githubusercontent.com/64727012/172078707-b1bc984e-2eb8-42c1-b56e-1f694d3c939e.png)

  * Example
    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #define MAX_VERTEX 30					// 헤드 포인터 배열의 최대 크기

    // 인접 리스트의 노드 구조를 구조체로 정의
    typedef struct graphNode {
      int vertex;							// 정점을 나타내는 데이터 필드
      struct graphNode* link;				// 다음 인접 정점을 연결하는 링크 필드
    } graphNode;

    // 그래프를 인접 리스트로 표현하기 위한 구조체 정의
    typedef struct graphType {
      int n;								// 그래프의 정점 개수
      graphNode* adjList_H[MAX_VERTEX];	// 그래프 정점에 대한 헤드 포인터 배열
    } graphType;

    // 공백 그래프를 생성하는 연산
    void createGraph(graphType* g) {
      int v;
      g->n = 0;							// 그래프의 정점 개수를 0으로 초기화
      for (v = 0; v<MAX_VERTEX; v++)
        g->adjList_H[v] = NULL;			// 그래프의 정점에 대한 헤드 포인터 배열을 NULL로 초기화
    }

    // 그래프 g에 정점 v를 삽입하는 연산
    void insertVertex(graphType* g, int v) {
      if (((g->n) + 1)>MAX_VERTEX) {
        printf("\n 그래프 정점의 개수를 초과하였습니다!");
        return;
      }
      g->n++;								// 그래프의 정점 개수 n을 하나 증가
    }

    // 그래프 g에 간선 (u, v)를 삽입하는 연산
    void insertEdge(graphType* g, int u, int v) {
      graphNode* node;

      // 간선 (u, v)를 삽입하기 위해 정점 u와 정점 v가 현재 그래프에 있는지 확인
      if (u >= g->n || v >= g->n) {
        printf("\n 그래프에 없는 정점입니다!");
        return;
      }
      node = (graphNode *)malloc(sizeof(graphNode));
      node->vertex = v;
      node->link = g->adjList_H[u];	// 삽입 간선에 대한 노드를 리스트의 첫 번째 노드로 연결
      g->adjList_H[u] = node;
    }

    // 그래프 g의 각 정점에 대한 인접 리스트를 출력하는 연산
    void print_adjList(graphType* g) {
      int i;
      graphNode* p;
      for (i = 0; i<g->n; i++) {
        printf("\n\t\t정점 %c의 인접 리스트", i + 65);
        p = g->adjList_H[i];
        while (p) {
          printf(" -> %c", p->vertex + 65); // 정점 0~3을 A~D로 출력
          p = p->link;
        }
      }
    }

    void main() {
      int i;
      graphType *G1, *G2, *G3, *G4;
      G1 = (graphType *)malloc(sizeof(graphType));
      G2 = (graphType *)malloc(sizeof(graphType));
      G3 = (graphType *)malloc(sizeof(graphType));
      G4 = (graphType *)malloc(sizeof(graphType));
      createGraph(G1); createGraph(G2); createGraph(G3); createGraph(G4);

      // 그래프 G1
      for (i = 0; i<4; i++)
        insertVertex(G1, i);	// G1의 정점 0~3 삽입
      insertEdge(G1, 0, 3);
      insertEdge(G1, 0, 1);
      insertEdge(G1, 1, 3);
      insertEdge(G1, 1, 2);
      insertEdge(G1, 1, 0);
      insertEdge(G1, 2, 3);
      insertEdge(G1, 2, 1);
      insertEdge(G1, 3, 2);
      insertEdge(G1, 3, 1);
      insertEdge(G1, 3, 0);
      printf("\n G1의 인접 리스트");
      print_adjList(G1);

      // 그래프 G2
      for (i = 0; i<3; i++)
        insertVertex(G2, i);	// G2의 정점 0~2 삽입
      insertEdge(G2, 0, 2);
      insertEdge(G2, 0, 1);
      insertEdge(G2, 1, 2);
      insertEdge(G2, 1, 0);
      insertEdge(G2, 2, 1);
      insertEdge(G2, 2, 0);
      printf("\n\n G2의 인접 리스트");
      print_adjList(G2);

      // 그래프 G3
      for (i = 0; i<4; i++)
        insertVertex(G3, i);	// G3의 정점 0~3 삽입
      insertEdge(G3, 0, 3);
      insertEdge(G3, 0, 1);
      insertEdge(G3, 1, 3);
      insertEdge(G3, 1, 2);
      insertEdge(G3, 2, 3);
      printf("\n\n G3의 인접 리스트");
      print_adjList(G3);

      // 그래프 G4
      for (i = 0; i<3; i++)
        insertVertex(G4, i);	// G4의 정점 0~2 삽입
      insertEdge(G4, 0, 2);
      insertEdge(G4, 0, 1);
      insertEdge(G4, 1, 2);
      insertEdge(G4, 1, 0);
      printf("\n\n G4의 인접 리스트");
      print_adjList(G4);

      getchar();
    }
    
    ```

#### Graph Traversal, Search
<strong>1. DFS (Depth First Search)</strong>
  * Adjacency Matrix => O(n^2) / Adjacency List => O(n+m) : n = vertex, m = edge
  * Implementation => Stack or Recursive

  ![image](https://user-images.githubusercontent.com/64727012/172079290-ca980998-c98d-48bc-b0ad-2ae3683ae253.png)

  * Example
    ```c
    #include <memory.h>
    #include <stdlib.h>
    #define MAX_VERTEX 10
    #define FALSE 0
    #define TRUE 1

    // 그래프에 대한 인접 리스트의 노드 구조 정의
    typedef struct graphNode {
      int vertex;
      struct graphNode* link;
    } graphNode;

    typedef struct graphType {
      int n;								// 정점 개수
      graphNode* adjList_H[MAX_VERTEX];	// 정점에 대한 인접 리스트의 헤드 노드 배열
      int visited[MAX_VERTEX];			// 정점에 대한 방문 표시를 위한 배열
    } graphType;

    // << 스택 (5장에서 설명한 연결 리스트를 이용한 스택 연산 수행:[예제 5-2]의 05~43행과 동일)
    typedef int element;

    typedef struct stackNode {
      int data;
      struct stackNode *link;
    } stackNode;

    stackNode* top;

    // 스택이 공백인지 확인하는 연산
    int isEmpty() {
      if (top == NULL) return 1;
      else return 0;
    }

    void push(int item) {
      stackNode* temp = (stackNode *)malloc(sizeof(stackNode));
      temp->data = item;
      temp->link = top;
      top = temp;
    }

    int pop() {
      int item;
      stackNode* temp = top;

      if (isEmpty()) {
        printf("\n\n Stack is empty !\n");
        return 0;
      }
      else {
        item = temp->data;
        top = temp->link;
        free(temp);
        return item;
      }
    } // 스택 >>

    // 깊이 우선 탐색을 위해 초기 공백 그래프를 생성하는 연산
    void createGraph(graphType* g) {
      int v;
      g->n = 0;							// 그래프의 정점 개수를 0으로 초기화
      for (v = 0; v<MAX_VERTEX; v++) {
        g->visited[v] = FALSE;			// 그래프의 정점에 대한 배열 visited를 FALSE로 초기화
        g->adjList_H[v] = NULL;			// 인접 리스트에 대한 헤드 노드 배열을 NULL로 초기화
      }
    }

    // 그래프 g에 정점 v를 삽입하는 연산 : [예제 8-2]의 26~32행과 동일
    void insertVertex(graphType* g, int v) {
      if (((g->n) + 1)>MAX_VERTEX) {
        printf("\n 그래프 정점의 개수를 초과하였습니다!");
        return;
      }
      g->n++;
    }

    // 그래프 g에 간선 (u, v)를 삽입하는 연산 : [예제 8-2]의 35~47행과 동일
    void insertEdge(graphType* g, int u, int v) {
      graphNode* node;
      if (u >= g->n || v >= g->n) {
        printf("\n 그래프에 없는 정점입니다!");
        return;
      }
      node = (graphNode *)malloc(sizeof(graphNode));
      node->vertex = v;
      node->link = g->adjList_H[u];
      g->adjList_H[u] = node;
    }

    // 그래프 g의 각 정점에 대한 인접 리스트를 출력하는 연산 : [예제 8-2]의 50~61행과 동일
    void print_adjList(graphType* g) {
      int i;
      graphNode* p;
      for (i = 0; i<g->n; i++) {
        printf("\n\t\t정점%c의 인접 리스트", i + 65);
        p = g->adjList_H[i];
        while (p) {
          printf(" -> %c", p->vertex + 65);
          p = p->link;
        }
      }
    }

    // 그래프 g에서 정점 v에 대한 깊이 우선 탐색 연산
    void DFS_adjList(graphType* g, int v) {
      graphNode* w;
      top = NULL;					// 스택의 top 설정
      push(v);					// 깊이 우선 탐색을 시작하는 정점 v를 스택에 push
      g->visited[v] = TRUE;		// 정점 v를 방문했으므로 해당 배열 값을 TRUE로 설정 
      printf(" %c", v + 65);

      // 스택이 공백이 아닌 동안 깊이 우선 탐색 반복
      while (!isEmpty()) {
        w = g->adjList_H[v];
        // 인접 정점이 있는 동안 수행
        while (w) {
          // 현재 정점 w를 방문하지 않은 경우 
          if (!g->visited[w->vertex]) {
            push(w->vertex);					// 현재 정점 W를 스택에 push
            g->visited[w->vertex] = TRUE;	// 정점 w에 대한 배열 값을 TRUE로 설정
            printf(" %c", w->vertex + 65);	// 정점 0~6을 A~G로 바꾸어서 출력
            v = w->vertex;
            w = g->adjList_H[v];
          }
          // 현재 정점 w가 이미 방문된 경우
          else w = w->link;
        }
        v = pop();// 현재 정점에서 순회를 진행할 인접 정점이 더 없는 경우에 스택 pop!
      } // 스택이 공백이면 깊이 우선 탐색 종료
    }

    void main() {
      int i;
      graphType *G7;
      G7 = (graphType *)malloc(sizeof(graphType));
      createGraph(G7);

      // 그래프 G7 구성
      for (i = 0; i<7; i++)
        insertVertex(G7, i);
      insertEdge(G7, 0, 2);
      insertEdge(G7, 0, 1);
      insertEdge(G7, 1, 4);
      insertEdge(G7, 1, 3);
      insertEdge(G7, 1, 0);
      insertEdge(G7, 2, 4);
      insertEdge(G7, 2, 0);
      insertEdge(G7, 3, 6);
      insertEdge(G7, 3, 1);
      insertEdge(G7, 4, 6);
      insertEdge(G7, 4, 2);
      insertEdge(G7, 4, 1);
      insertEdge(G7, 5, 6);
      insertEdge(G7, 6, 5);
      insertEdge(G7, 6, 4);
      insertEdge(G7, 6, 3);
      printf("\n G9의 인접 리스트 ");
      print_adjList(G7);

      printf("\n\n///////////////\n\n깊이 우선 탐색 >> ");
      DFS_adjList(G7, 0);	// 0번 정점인 정점 A에서 깊이 우선 탐색 시작

      getchar();
    }
    ```


<strong>2. BFS (Breadth First Search)</strong>
  * Adjacency Matrix => O(n^2) / Adjacency List => O(n+m) : n = vertex, m = edge
  * Implementation => Queue
  
  ![image](https://user-images.githubusercontent.com/64727012/172079294-d1b412e2-8a49-419a-956b-73df69c4bb8e.png)
  * Example
    ```c
    #include<stdio.h>
    #define MAX_VERTEX 30

    typedef char Datatype;

    typedef struct QueueNode {
        Datatype data;
        struct QueueNode* next;
    }Node;

    typedef struct ArrayQueue {
        int index;
        Node* rear;
        Node* front;
    }Queue;

    Queue* init() {
        Queue* temp = (Queue*)malloc(sizeof(Queue));
        temp->front = (Node*)malloc(sizeof(Node));
        temp->rear = (Node*)malloc(sizeof(Node));
        temp->index = -1;
        return temp;
    }
    int isEmpty(Queue* pQueue) {
        if (pQueue->index == -1) {
            return 1;
        }
        else return 0;
    }

    void qInsertRear(Queue* pQueue, char data) {
        Node* temp = (Node*)malloc(sizeof(Node));
        Node* tmp = pQueue->front->next;
        temp->data = data;
        temp->next = NULL;

        if (isEmpty(pQueue) == 1) {
            pQueue->front->next = temp;
            pQueue->rear->next = temp;
            temp->next = temp;
        }
        else {
            pQueue->rear->next->next = temp;
            temp->next = pQueue->front->next;
            pQueue->rear->next = temp;
        }
        pQueue->index++;
    }

    void qInsertFront(Queue* pQueue, char data) {
        Node* temp = (Node*)malloc(sizeof(Node));

        temp->data = data;
        temp->next = NULL;

        if (isEmpty(pQueue) == 1) {
            pQueue->front->next = temp;
            pQueue->rear->next = temp;
            temp->next = temp;
        }
        else {
            temp->next = pQueue->front->next;
            pQueue->rear->next->next = temp;
            pQueue->front->next = temp;
        }
        pQueue->index++;
    }

    void qDeleteFront(Queue* pQueue) {
        if (isEmpty(pQueue) == 1)return 0;
        else {
            Node* tempF = (Node*)malloc(sizeof(Node));
            tempF = pQueue->front->next;
            pQueue->rear->next->next = pQueue->front->next->next;
            free(tempF);
            pQueue->front->next = pQueue->rear->next->next;
        }
        pQueue->index--;
    }

    void qDeleteRear(Queue* pQueue) {

        if (isEmpty(pQueue) == 1)return 0;
        else {
            Node* tempR = (Node*)malloc(sizeof(Node));
            Node* tmp = (Node*)malloc(sizeof(Node));
            tmp = pQueue->rear->next;
            tempR = pQueue->front->next;

            for (int i = 0; i < pQueue->index - 1; i++) {
                tempR = tempR->next;
            }
            tempR->next = pQueue->front->next;
            pQueue->rear->next = tempR;
            free(tmp);
        }
        pQueue->index--;
    }
    Datatype getQueueFront(Queue* pQueue) {
        return pQueue->front->next->data;
    }
    Datatype getQueueRear(Queue* pQueue) {
        return pQueue->rear->next->data;
    }

    typedef struct graphNode {
      int vertex;
      struct graphNode* link;
    }graphNode;

    typedef struct graphType {
      int n;
      graphNode* adjList_H[MAX_VERTEX];

    }graphType;

    void createGraph(graphType* g) {
      g->n = 0;
      for (int v = 0; v < MAX_VERTEX; v++)
        g->adjList_H[v] = NULL;
    }

    void insertVertex(graphType* g, int u) {
      if (((g->n) + 1) > MAX_VERTEX) {
        printf("\n 그래프 정점의 개수를 초과하였습니다.");
        return;
      }
      g->n++;
    }

    void insertEdge(graphType* g, int u, int v) {//u,v는 정점
      graphNode* node;

      if (u >= g->n || v >= g->n) {
        printf("\n 그래프에 없는 정점입니다!");
        return;
      }
      node = (graphNode*)malloc(sizeof(graphNode));
      node->vertex = v;
      node->link = g->adjList_H[u];
      g->adjList_H[u] = node; 
    }

    void print_adjList(graphType* g) {
      graphNode* p;
      for (int i = 0; i < g->n; i++) {
        printf("\n\t\t정점 %c의 인접 리스트", i + 65);
        p = g->adjList_H[i];
        while (p) {
          printf(" -> %c", p->vertex + 65);
          p = p->link;
        }
      }
    }

    void BFS_adjList(graphType* g, int v) {

        int visited[MAX_VERTEX];
        int k = 0;

        for (int i = 0; i <= g->n; i++) {
            visited[i] = 0;
        }
        Queue* q = init();
        visited[v] = 1;
        printf("%c ", v + 65);
        graphNode* p;
        p = g->adjList_H[v];

        while (1) {
            while (1) {
                if (visited[p->vertex] == 0) {
                    qInsertRear(q, p->vertex + 65);
                    visited[p->vertex] = 1;
                }
                if (p->link == NULL)
                    break;
                p = p->link;
            }
            p = g->adjList_H[v++];
            if (v > g->n) {
                p = g->adjList_H[0];
                v = 0;
            }
            k++;
            if (k > g->n)
                break;
        }
        int count = q->index;
        for (int i = 0; i <= count; i++) {
            printf("%c ", getQueueFront(q));
            qDeleteFront(q);
        }
    }

    void main() {
      int i;
      graphType* G9;
      G9 = (graphType*)malloc(sizeof(graphType));
      createGraph(G9);

      // 그래프 G9 구성
      for (i = 0; i < 7; i++)
        insertVertex(G9, i);
      insertEdge(G9, 0, 2);
      insertEdge(G9, 0, 1);

      insertEdge(G9, 1, 4);
      insertEdge(G9, 1, 3);
      insertEdge(G9, 1, 0);

      insertEdge(G9, 2, 4);
      insertEdge(G9, 2, 0);

      insertEdge(G9, 3, 6);
      insertEdge(G9, 3, 1);

      insertEdge(G9, 4, 6);
      insertEdge(G9, 4, 2);
      insertEdge(G9, 4, 1);

      insertEdge(G9, 5, 6);

      insertEdge(G9, 6, 5);
      insertEdge(G9, 6, 4);
      insertEdge(G9, 6, 3);
      printf("\n G9의 인접 리스트 ");
      print_adjList(G9);

      printf("\n\n///////////////\n\n너비 우선 탐색 >> ");
      BFS_adjList(G9, 0);      // 0번 정점인 정점 A에서 너비 우선 탐색 시작

      getchar();
    }
    ```
### Spanning Tree
* A tree with n vertices and n-1 edges
* Tree is linked grahp that has no cycle

  ![image](https://user-images.githubusercontent.com/64727012/172079914-571f4ee3-344a-4277-a9fc-1e885cb2a871.png)

#### MST (Minimum cost Spanning Tree)
* In undirected weighted graph, spanning tree that sum of weight of edges consisting of spanning tree is minimum
* Algorithm => Kruskal, Prim

<strong>1. Kruskal Algorithm</strong>
  * By removing high weighted edges or inserting low weighted edges, way to make MST
    * Require sorting weighted edges
  * <strong>Union-find</strong> Algorithm
    * Find which set the element belongs to
    * In MST algorithm of Kruskal, use to inspect cycle
    * O(m log m) : m = edge

  ![image](https://user-images.githubusercontent.com/64727012/172081715-3ca040f1-f99a-467e-bf46-05c499fdb625.png)
  * Example
    ```c
    #include <stdio.h>
    #include <stdlib.h>

    #define TRUE 1
    #define FALSE 0

    #define MAX_VERTICES 100
    #define INF 1000

    int parent[MAX_VERTICES];		// 부모 노드
                  // 초기화
    void set_init(int n)
    {
      for (int i = 0; i<n; i++) 
        parent[i] = -1;
    }
    // curr가 속하는 집합을 반환한다.
    int set_find(int curr)
    {
      if (parent[curr] == -1)
        return curr; 			// 루트 
      while (parent[curr] != -1) curr = parent[curr];
      return curr;
    }

    // 두개의 원소가 속한 집합을 합친다.
    void set_union(int a, int b)
    {
      int root1 = set_find(a);	// 노드 a의 루트를 찾는다. 
      int root2 = set_find(b);	// 노드 b의 루트를 찾는다. 
      if (root1 != root2) 	// 합한다. 
        parent[root1] = root2;
    }

    struct Edge {			// 간선을 나타내는 구조체
      int start, end, weight;
    };

    typedef struct GraphType {
      int n;	// 간선의 개수
      struct Edge edges[2 * MAX_VERTICES];
    } GraphType;

    // 그래프 초기화 
    void graph_init(GraphType* g)
    {
      g->n = 0;
      for (int i = 0; i < 2 * MAX_VERTICES; i++) {
        g->edges[i].start = 0;
        g->edges[i].end = 0;
        g->edges[i].weight = INF;
      }
    }
    // 간선 삽입 연산
    void insert_edge(GraphType* g, int start, int end, int w)
    {
      g->edges[g->n].start = start;
      g->edges[g->n].end = end;
      g->edges[g->n].weight = w;
      g->n++;
    }
    // qsort()에 사용되는 함수
    int compare(const void* a, const void* b)
    {
      struct Edge* x = (struct Edge*)a;
      struct Edge* y = (struct Edge*)b;
      return (x->weight - y->weight);      // 오름 차순
    //	return (y->weight - x->weight);      // 내림 차순
    }

    void print_sort(GraphType* g, int n)
    {
      for (int i = 0; i<n; i++) {
            printf("( %2d,", g->edges[i].start);
            printf("%2d,", g->edges[i].end);
            printf("%2d ) ", g->edges[i].weight);
      }
      printf("\n");
    }

    void print_parent(int n)
    {
      for (int i = 0; i<n; i++) {
            printf("%2d", parent[i]);
      }
      printf("\n");
    }

    // kruskal의 최소 비용 신장 트리 프로그램
    void kruskal(GraphType *g)
    {
      int edge_accepted = 0;	// 현재까지 선택된 간선의 수	
      int uset, vset;			// 정점 u와 정점 v의 집합 번호
      struct Edge e;


      set_init(g->n);				// 집합 초기화
      print_sort(g, g->n);
      qsort(g->edges, g->n, sizeof(struct Edge), compare);
        print_sort(g, g->n);
      printf("크루스칼 최소 신장 트리 알고리즘 \n");
      int i = 0;
      print_parent(g->n);
      while (edge_accepted < (g->n - 1))	// 간선의 수 < (n-1)
      {
        e = g->edges[i];
        uset = set_find(e.start);		// 정점 u의 집합 번호 
        vset = set_find(e.end);		// 정점 v의 집합 번호
        if (uset != vset) {			// 서로 속한 집합이 다르면
          printf("간선 (%d,%d) %d 선택\n", e.start, e.end, e.weight);
          edge_accepted++;
          set_union(uset, vset);	// 두개의 집합을 합친다.
          print_parent(g->n);
        }
        i++;
      }
    }
    int main(void)
    {
      GraphType *g;
      g = (GraphType *)malloc(sizeof(GraphType));
      graph_init(g);

      insert_edge(g, 0, 1, 3);
      insert_edge(g, 0, 2, 17);
      insert_edge(g, 0, 3, 6);
      insert_edge(g, 1, 3, 5);
      insert_edge(g, 1, 6, 12);
      insert_edge(g, 2, 4, 10);
      insert_edge(g, 2, 5, 8);
      insert_edge(g, 3, 4, 9);
      insert_edge(g, 4, 5, 4);
      insert_edge(g, 4, 6, 2);
      insert_edge(g, 5, 6, 14);

      kruskal(g);
      free(g);
      return 0;
    }
    ```

<strong>2. Prim Algorithm</strong>
  * Not sorting edges, way to expand tree from one of edges
  * Among all edges of selected vertices, expand tree by inserting the lowest edge
    * A edge to make cycle cannot insert, so, in this case, select the next lowest edge
  * Continue until number of edges is n-1
  * O(n^2) : n = vertex
  
  ![image](https://user-images.githubusercontent.com/64727012/172084038-1e466617-f4e5-4a58-ac7c-d53f8ed8de10.png)
  * Example
    ```c
    #include <stdio.h>
    #include <stdlib.h>

    #define TRUE 1
    #define FALSE 0
    #define MAX_VERTICES 100
    #define INF 1000L

    typedef struct GraphType {
      int n;	// 정점의 개수
      int weight[MAX_VERTICES][MAX_VERTICES];
    } GraphType;

    int selected[MAX_VERTICES];
    int distance[MAX_VERTICES];

    // 최소 dist[v] 값을 갖는 정점을 반환
    int get_min_vertex(int n)
    {
      int v, i;
      for (i = 0; i <n; i++)
        if (!selected[i]) {
          v = i;
          break;
        }
      for (i = 0; i < n; i++)
        if (!selected[i] && (distance[i] < distance[v])) v = i;
      return (v);
    }
    //
    void prim(GraphType* g, int s)
    {
      int i, u, v;

      for (u = 0; u<g->n; u++)
        distance[u] = INF;
      distance[s] = 0;
      for (i = 0; i<g->n; i++) {
        u = get_min_vertex(g->n);
        selected[u] = TRUE;
        if (distance[u] == INF) return;
        printf("정점 %d 추가\n", u);
        for (v = 0; v<g->n; v++)
          if (g->weight[u][v] != INF)
            if (!selected[v] && g->weight[u][v]< distance[v])
              distance[v] = g->weight[u][v];
      }
    }

    int main(void)
    {
    /*	GraphType g = { 7, 
      {{ 0, 29, INF, INF, INF, 10, INF },
      { 29,  0, 16, INF, INF, INF, 15 },
      { INF, 16, 0, 12, INF, INF, INF },
      { INF, INF, 12, 0, 22, INF, 18 },
      { INF, INF, INF, 22, 0, 27, 25 },
      { 10, INF, INF, INF, 27, 0, INF },
      { INF, 15, INF, 18, 25, INF, 0 } }
      }; */

      GraphType g = { 7, 
      {{  0,   3,  17,   6, INF, INF, INF },
      {   3,   0, INF,   5, INF, INF,  12 },
      {  17, INF,   0, INF,  10,   8, INF },
      {   6,   5, INF,   0,   9, INF, INF },
      { INF, INF,  10,   9,   0,   4,   2 },
      { INF, INF,   8, INF,   4,   0,  14 },
      { INF,  12, INF, INF,   2,  14,  0 }}
      };
      prim(&g, 0);
      return 0;
    }
    
    ```

### Shortest Path Algorithm
<strong>1. Dijkstra Algorithm</strong>
  * Algorithm finding shortest path from Source s to all other vertices<strong> in graph that all edge's weight are positive </strong>
  * Both undirect and direct graph can be applied 
  * V=> Set of vertices
  * S=> Set of vertices that found the current shortest path from source s
  * D[v]=> distance of the shortest path from s to vertex V through the vertices belonging to S
    * <strong>D[w] = min {D[w], D[u] + weight(u, w)}
  * Adjacency Matrix => O(n^2)
  * Adjacency List => (D[] => Array)-O(n^2), (D[] => minheap)-O(m log n) : m = edge, n= vertex</strong> 
  
  ![image](https://user-images.githubusercontent.com/64727012/172095146-a04f15f3-74aa-44b0-a674-153ef746b365.png)
  * Example
    ```c
    #include <stdio.h>
    #include <limits.h>

    #define TRUE  1
    #define FALSE 0
    #define MAX_VERTICES 5						// 그래프의 정점 개수
    #define INF  10000

    int weight[MAX_VERTICES][MAX_VERTICES] = {	// 그래프 G11의 가중치 인접행렬
      { 0, 10, 5, INF, INF },
      { INF, 0, 2, 1, INF },
      { INF, 3, 0, 9, 2 },
      { INF, INF, INF, 0, 4 },
      { 7, INF, INF, 6, 0 },
    };
    int distance[MAX_VERTICES];			// 시작 정점으로부터의 최단 경로 길이 저장
    int S[MAX_VERTICES];				// 정점의 집합 S

    // 최소 거리를 갖는 다음 정점을 찾는 연산
    int nextVertex(int n) {
      int i, min, minPos;
      min = INT_MAX;
      minPos = -1;
      for (i = 0; i< n; i++)
        if ((distance[i] < min) && !S[i]) {
          min = distance[i];
          minPos = i;
        }
      return minPos;
    }

    // 최단 경로 구하는 과정을 출력하는 연산
    int printStep(int step) {
      int i;
      printf("\n %3d 단계 : S={", step);
      for (i = 0; i < MAX_VERTICES; i++)
        if (S[i] == TRUE)
          printf("%3c", i + 65);

      if (step<1) printf(" } \t\t\t");
      else if (step<4) printf(" } \t\t");
      else printf(" } \t");
      printf(" distance :[ ");
      for (i = 0; i < MAX_VERTICES; i++)
        if (distance[i] == 10000)
          printf("%4c", '*');
        else printf("%4d", distance[i]);
        printf("%4c", ']');
        return ++step;
    }

    void Dijkstra_shortestPath(int start, int n) {
      int i, u, w, step = 0;

      for (i = 1; i < n; i++) {	// 초기화
        distance[i] = weight[start][i];
        S[i] = FALSE;
      }

      S[start] = TRUE;			// 시작 정점을 집합 S에 추가
      distance[start] = 0;		// 시작 정점의 최단경로를 0으로 설정

      step = printStep(0);		// 0단계 상태를 출력

      for (i = 0; i < n - 1; i++) {
        u = nextVertex(n);		// 최단 경로를 만드는 다음 정점 u 찾기
        S[u] = TRUE;			// 정점 u를 집합 S에 추가
        for (w = 0; w < n; w++)
          if (!S[w])			// 집합 S에 포함되지 않은 정점 중에서
            if (distance[u] + weight[u][w] < distance[w])
              distance[w] = distance[u] + weight[u][w];	// 경로 길이 수정

        step = printStep(step);	// 현재 단계 출력
      }
    }
    void main(void) {
      int i, j;
      printf("\n ********** 가중치 인접 행렬 **********\n\n");
      for (i = 0; i < MAX_VERTICES; i++) {
        for (j = 0; j < MAX_VERTICES; j++) {
          if (weight[i][j] == 10000)
            printf("%4c", '*');
          else printf("%4d", weight[i][j]);
        }
        printf("\n\n");
      }

      printf("\n ********** 다익스트라 최단 경로 구하기 **********\n\n");
      Dijkstra_shortestPath(0, MAX_VERTICES);

      getchar();
    }
    ```


<strong>2. Floyd Algorithm</strong>
  * <strong>Calculate all Shortest path between all vertices
  * O(n^3) : n = vertex</strong>
  * Example
    ```c
    #include <stdio.h>
    #include <limits.h>

    #define MAX_VERTICES 5						// 그래프의 정점 개수
    #define INF  10000

    int weight[MAX_VERTICES][MAX_VERTICES] = {	// 그래프 G11의 가중치 인접행렬
      { 0,10,5,INF,INF },
      { INF,0,2,1,INF },
      { INF, 3,0,9,2 },
      { INF,INF,INF,0,4 },
      { 7,INF,INF,6,0 },
    };

    int A[MAX_VERTICES][MAX_VERTICES];			// k-최단 경로 배열

    // 최단 경로를 구하는 과정을 출력하는 연산
    void printStep(int step) {					
      int i, j;
      printf("\n A%d : ", step);
      for (i = 0; i < MAX_VERTICES; i++) {
        printf("\t");
        for (j = 0; j < MAX_VERTICES; j++) {
          if (A[i][j] == 10000)
            printf("%4c", '*');
          else printf("%4d", A[i][j]);
        }
        printf("\n\n");
      }
    }

    void Floyd_shortestPath(int n) {
      int v, w, k, step = -1;

      for (v = 0; v < n; v++)		// 초기화
        for (w = 0; w < n; w++)
          A[v][w] = weight[v][w];

      printStep(step);

      for (k = 0; k < n; k++) {
        for (v = 0; v < n; v++)
          for (w = 0; w < n; w++)
            if (A[v][k] + A[k][w] < A[v][w])
              A[v][w] = A[v][k] + A[k][w];
        printStep(++step);
      }
    }

    void main(void) {
      int i, j;
      printf("\n ********** 가중치 인접 행렬 **********\n\n");
      for (i = 0; i < MAX_VERTICES; i++) {
        for (j = 0; j < MAX_VERTICES; j++) {
          if (weight[i][j] == 10000)
            printf("%4c", '*');
          else printf("%4d", weight[i][j]);
        }
        printf("\n\n");
      }

      printf("\n ********* 플로이드 최단 경로 구하기 *********\n\n");
      Floyd_shortestPath(MAX_VERTICES);

      getchar();
    }
    ```
  

## Sort

### Selection sort
* O(n^2)
* Code
  ```cpp
  void selectionSort(int arr[], int n){
      for(int i=n-1; i>=0 ; i--){
          int max = 0;
          for(int j=0; j<=i; j++){
              if(arr[max] < arr[j])
                  max = j;
          }
          int temp = arr[max];
          arr[max] = arr[i];
          arr[i] = temp;
      }
  }
  ```

### Bubble Sort
* O(n^2) / In the best case, O(n)
* Code
  ```cpp
  void bubbleSort(int arr[], int n){
      int sorted = 1;
      for(int i=n-1; i>0; i--){
          for(int j=0; j<i; j++){
              if(arr[j] > arr[j+1]){
                  int temp = arr[j];
                  arr[j] = arr[j+1];
                  arr[j+1] = temp;
                  sorted = 0;
              }
          }
          if(sorted == 1)
              break;
      }
  }
  ```

### Quick Sort
* O(n log n) / In the worst case, O(n^2)
* Code
  ```cpp
  int partition(int num[], int left, int right){
      //피봇을 랜덤하게 선택
      int pivotIndex = rand()%(right+1-left)+left;
      int pivot=num[pivotIndex];

      //피봇을 맨 왼쪽과 교환한다.
      int tmp = num[pivotIndex];
      num[pivotIndex] = num[left];
      num[left] = tmp;

      int low=left+1;
      int high=right;

      while(low<=high){
          //pivot보다 큰 low인덱스를 찾음
          while(low<=right && pivot>=num[low]){
              low++;
          }
          //pivot보다 작은 high인덱스를 찾음
          while(high>=left+1 && pivot<=num[high]){
              high--;
          }
          //low와 high를 바꿈
          if(low<=high){
              int temp=num[low];
              num[low]=num[high];
              num[high]=temp;
          }
      }
      //high가 멈춘곳과 pivot값을 교환함
      int temp=num[left];
      num[left]=num[high];
      num[high]=temp;

      //high값을 기준으로 분리해야하므로 high리턴
      return high;
  }

  void quickSort(int num[], int left, int right){
      if(left<=right){
          int pivot=partition(num,left,right);
          quickSort(num,left,pivot-1);
          quickSort(num,pivot+1,right);
      }
  }
  ```

### Shell Sort
* average : O(n^1.5) / worst : O(n^2)
* Code
  ```c
  void intervalSort(element a[], int begin, int end, int interval) {
    int i, j;
    element item;
    for (i = begin + interval; i <= end; i = i + interval) {
      item = a[i];
      for (j = i - interval; j >= begin && item<a[j]; j = j - interval)
        a[j + interval] = a[j];
      a[j + interval] = item;
    }
  }

  void shellSort(int a[], int size) {
    int i, t, interval;
    printf("\n정렬할 원소 : ");
    for (t = 0; t<size; t++)  printf("%d ", a[t]);
    printf("\n\n<<<<<<<<<< 셸 정렬 수행 >>>>>>>>>>\n");
    interval = size / 2;
    while (interval >= 1) {
      for (i = 0; i<interval; i++)  intervalSort(a, i, size - 1, interval);
      printf("\n interval=%d >> ", interval);
      for (t = 0; t<size; t++) printf("%d ", a[t]);
      printf("\n");
      interval = interval / 2;
    }
  }
  ```

### Merge Sort
* O(n log n)
* Code
  ```cpp
  void merge(int a[], int first, int mid, int last)
  {
      int i = first;
      int j = mid + 1;
      int k = first;

      while (i <= mid && j <= last){
          if (a[i] <= a[j]){
              temp[k] = a[i];
              i++;
          }
          else{
              temp[k] = a[j];
              j++;
          }
          k++;
      }

      if (i > mid){
          for (int t = j; t <= last; t++){
              temp[k] = a[t];
              k++;
          }
      }
      else{
          for (int t = i; t <= mid; t++){
              temp[k] = a[t];
              k++;
          }
      }

      for (int t = first; t <= last; t++)
          a[t] = temp[t];
  }

  void mergeSort(int a[], int first, int last){
      if(first<last){
          int mid = (first+last)/2;
          mergeSort(a, first, mid);
          mergeSort(a, mid+1, last);
          merge(a, first, mid, last);
      }

  }
  ```
### Radix Sort
* O(d(n+r))
* Code
  ```cpp
  void radixSort(int a[], int n){
      queue<int>Q[10];
      int max = a[0], radix = 1;

      for(int i=0; i<n; i++){
          if(max < a[i])
              max = a[i];
      }

      while(1){//최댓값찾기
          if(radix >= max)
              break;
          radix *= 10;
      }

      for(int i=1; i<=radix; i*=10){
          //분할
          for(int j=0; j<n; j++){
              int k;
              if(a[j]<i)
                  k=0;
              else
                  k=(a[j]/i)%10;
              Q[k].push(a[j]);            
          }
          //모으기
          int idx = 0;
          for(int j=0; j<10; j++){
              while(Q[j].empty()==0){
                  a[idx] = Q[j].front();
                  Q[j].pop();
                  idx++;
              }
          }
      }  
  }
  ```


## Search
* Comparison Search Method => Sequencial search, Binary search, Tree search
* Non-Comparison Method => Hashing

### Sequential Search (Linear Search)
* O(n)
### Binary Search
* O(log n)
#### Binary Tree Search
* To use binary tree as data structure for searching, define the node's position according to element's key value
  * Left subtree's key value < root's key value < right subtree's key value 
* O(log n)
* Example
  ```c
  #include<stdio.h> 
  #include<stdlib.h> 

  typedef char element;		// 이진 탐색 트리 element의 자료형을 char로 정의
  typedef struct treeNode {
    char key;				// 데이터 필드
    struct treeNode* left;	// 왼쪽 서브 트리 링크 필드
    struct treeNode* right;	// 오른쪽 서브 트리 링크 필드
  } treeNode;

  // 이진 탐색 트리에서 키값이 x인 노드의 위치를 탐색하는 연산
  treeNode* searchBST(treeNode* root, char x) {
    treeNode* p;
    p = root;
    while (p != NULL) {
      if (x < p->key) p = p->left;
      else if (x == p->key) return p;
      else p = p->right;
    }
    printf("\n 찾는 키가 없습니다!");
    return p;
  }

  // 포인터 p가 가리키는 노드와 비교하여 노드 x를 삽입하는 연산
  treeNode* insertNode(treeNode *p, char x) {
    treeNode *newNode;
    if (p == NULL) {
      newNode = (treeNode*)malloc(sizeof(treeNode));
      newNode->key = x;
      newNode->left = NULL;
      newNode->right = NULL;
      return newNode;
    }
    else if (x < p->key)  p->left = insertNode(p->left, x);
    else if (x > p->key)  p->right = insertNode(p->right, x);
    else  printf("\n 이미 같은 키가 있습니다! \n");

    return p;
  }

  // 루트 노드부터 탐색하여 키값과 같은 노드를 찾아 삭제하는 연산
  void deleteNode(treeNode *root, element key) {
    treeNode *parent, *p, *succ, *succ_parent;
    treeNode *child;

    parent = NULL;
    p = root;
    while ((p != NULL) && (p->key != key)) {  // 삭제할 노드의 위치 탐색
      parent = p;
      if (key < p->key) p = p->left;
      else p = p->right;
    }

    // 삭제할 노드가 없는 경우
    if (p == NULL) {
      printf("\n 찾는 키가 이진 트리에 없습니다!!");
      return;
    }

    // 삭제할 노드가 단말 노드인 경우
    if ((p->left == NULL) && (p->right == NULL)) {
      if (parent != NULL) {
        if (parent->left == p) parent->left = NULL;
        else parent->right = NULL;
      }
      else root = NULL;
    }

    // 삭제할 노드가 자식 노드를 한 개 가진 경우
    else if ((p->left == NULL) || (p->right == NULL)) {
      if (p->left != NULL) child = p->left;
      else child = p->right;

      if (parent != NULL) {
        if (parent->left == p) parent->left = child;
        else parent->right = child;
      }
      else root = child;
    }

    // 삭제할 노드가 자식 노드를 두 개 가진 경우
    else {
      succ_parent = p;
      succ = p->left;
      while (succ->right != NULL) { // 왼쪽 서브 트리에서 후계자 찾기
        succ_parent = succ;
        succ = succ->right;
      }
      if (succ_parent->left == succ)  succ_parent->left = succ->left;
      else succ_parent->right = succ->left;
      p->key = succ->key;
      p = succ;
    }
    free(p);
  }

  // 이진 탐색 트리를 중위 순회하면서 출력하는 연산
  void displayInorder(treeNode* root){
    if (root) {
      displayInorder(root->left);
      printf("%c_", root->key);
      displayInorder(root->right);
    }
  }

  void menu() {
    printf("\n*---------------------------*");
    printf("\n\t1 : 트리 출력");
    printf("\n\t2 : 문자 삽입");
    printf("\n\t3 : 문자 삭제");
    printf("\n\t4 : 문자 검색");
    printf("\n\t5 : 종료");
    printf("\n*---------------------------*");
    printf("\n메뉴입력 >> ");
  }

  int main() {
    treeNode* root = NULL;
    treeNode* foundedNode = NULL;
    char choice, key;

    // [그림 7-38]과 같은 초기 이진 탐색 트리를 구성하고 
    // 노드 G를 루트 노드 포인터 root로 지정
    root = insertNode(root, 'G');
    insertNode(root, 'I');
    insertNode(root, 'H');
    insertNode(root, 'D');
    insertNode(root, 'B');
    insertNode(root, 'M');
    insertNode(root, 'N');
    insertNode(root, 'A');
    insertNode(root, 'J');
    insertNode(root, 'E');
    insertNode(root, 'Q');

    while (1) {
      menu();
      scanf(" %c", &choice);

      switch (choice - '0') {
      case 1:	printf("\t[이진 트리 출력]  ");
        displayInorder(root);  printf("\n");
        break;

      case 2:	printf("삽입할 문자를 입력하세요 : ");
        scanf(" %c", &key);
        insertNode(root, key);
        break;

      case 3:	printf("삭제할 문자를 입력하세요 : ");
        scanf(" %c", &key);
        deleteNode(root, key);
        break;

      case 4: printf("찾을 문자를 입력하세요 : ");
        scanf(" %c", &key);
        foundedNode = searchBST(root, key);
        if (foundedNode != NULL)
          printf("\n %c를 찾았습니다! \n", foundedNode->key);
        else  printf("\n 문자를 찾지 못했습니다. \n");
        break;

      case 5: 	return 0;

      default: printf("없는 메뉴입니다. 메뉴를 다시 선택하세요! \n");
        break;
      }
    }
  }
  ```
#### AVL Tree
* The height difference of left and right subtree of all nodes is 1 or less (-1,0,1)
* If the tree become unbalance, nodes are relocated
* O(log n)
* Example
  ```c
  #include <stdio.h>
  #include <stdlib.h>
  #include <memory.h>
  #include <math.h>

  typedef struct avl_node {
    struct avl_node* left_child, * right_child;  /* Subtrees. */
    int data;                   /* Pointer to data. */
  } AVL_node;
  AVL_node* root;
  // 오른쪽 회전 함수
  AVL_node* rotate_right(AVL_node* parent)
  {
    AVL_node* child = parent->left_child;
    parent->left_child = child->right_child;
    child->right_child = parent;
    return child;
  }
  // 왼쪽 회전 함수
  AVL_node* rotate_left(AVL_node* parent)
  {
    AVL_node* child = parent->right_child;
    parent->right_child = child->left_child;
    child->left_child = parent;
    return child;
  }
  // 오른쪽-왼쪽 회전 함수
  AVL_node* rotate_right_left(AVL_node* parent)
  {
    AVL_node* child = parent->right_child;
    parent->right_child = rotate_right(child);
    return rotate_left(parent);
  }
  // 왼쪽-오른쪽 회전 함수
  AVL_node* rotate_left_right(AVL_node* parent)
  {
    AVL_node* child = parent->left_child;
    parent->left_child = rotate_left(child);
    return rotate_right(parent);
  }
  // 트리의 높이를 반환
  int get_height(AVL_node* node)
  {
    int height = 0;
    if (node != NULL)
      height = 1 + fmax(get_height(node->left_child), get_height(node->right_child));
    return height;
  }
  // 노드의 균형인수를 반환
  int get_height_diff(AVL_node* node)
  {
    if (node == NULL) return 0;
    return get_height(node->left_child) - get_height(node->right_child);
  }

  // 트리를 균형트리로 만든다
  AVL_node* rebalance(AVL_node** node)
  {
    int height_diff = get_height_diff(*node);            // 균형 인수 계산
    // 균형 인수가 +2 이상이면 LL 상태 또는 LR 상태
    if (height_diff > 1) {        // 왼쪽 서브 트리 방향으로 높이가 2 이상 크다면,
      if (get_height_diff((*node)->left_child) > 0)
        *node = rotate_right(*node);            // LL
      else
        *node = rotate_left_right(*node);       // LR
    }
    // 균형 인수가 -2 이상이면 RR 상태 또는 RL 상태
    else if (height_diff < -1) {     // 오른쪽 서브 트리 방향으로 높이가 2 이상 크다면,
      if (get_height_diff((*node)->right_child) < 0)
        *node = rotate_left(*node);             // RR
      else
        *node = rotate_right_left(*node);       // RL
    }
    return *node;
  }
  // AVL 트리의 삽입 연산
  AVL_node* avl_add(AVL_node** root, int new_key)
  {
    if (*root == NULL) {
      *root = (AVL_node*)malloc(sizeof(AVL_node));
      if (*root == NULL) {
        fprintf(stderr, "메모리 할당 에러\n");
        exit(1);
      }
      (*root)->data = new_key;
      (*root)->left_child = (*root)->right_child = NULL;
    }
    else if (new_key < (*root)->data) {
      (*root)->left_child = avl_add(&((*root)->left_child), new_key);
      *root = rebalance(root);
    }
    else if (new_key > (*root)->data) {
      (*root)->right_child = avl_add(&((*root)->right_child), new_key);
      (*root) = rebalance(root);
    }
    else {
      fprintf(stderr, "중복된 키 에러\n");
      exit(1);
    }
    return *root;
  }
  //AVL 트리의 삭제함수
  AVL_node* avl_delete(AVL_node* root, int key)
  {
    AVL_node* parent, * p, * succ, * succ_parent;
    AVL_node* child, * temp;

    parent = NULL;
    p = root;
    //삭제할 노드의 위치 탐색
    while ((p != NULL) && (p->data != key)) {
      parent = p;
      if (key < p->data) p = p->left_child;
      else p = p->right_child;
    }
    //삭제할 노드가 없는경우
    if (p == NULL) {
      printf("\nNot found\n");
      return root;
    }
    temp = root;
    //삭제할 노드가 단말인경우
    if ((p->left_child == NULL) && (p->right_child == NULL)) {
      if (parent != NULL) {
        if (parent->left_child == p) parent->left_child = NULL;
        else parent->right_child = NULL;
      }
      else root = NULL;
    }
    //삭제할 노드가 자식 노드를 한개 가진경우
    else if ((p->left_child == NULL) || (p->right_child == NULL)) {
      if (p->left_child != NULL) child = p->left_child; //왼쪽만 있는경우
      else child = p->right_child; //오른쪽만 있는경우

      if (parent != NULL) {
        if (parent->left_child == p) parent->left_child = child;
        else parent->right_child = child;
      }
      else root = child;
    }
    //삭제할 노드가 자식 노드를 두개 가진경우
    else {
      succ_parent = p;
      succ = p->left_child;
      while (succ->right_child != NULL) {
        succ_parent = succ;
        succ = succ->right_child;
      }
      if (succ_parent->left_child == succ)succ_parent->left_child = succ->left_child;
      else succ_parent->right_child = succ->left_child;
      p->data = succ->data;
      p = succ;
    }
    free(p);
    int balance = get_height_diff(root);

    while (root != NULL) {
      balance = get_height_diff(root);
      if (balance <= 1 && balance >= -1) {
        if ((root != NULL) && (root->data != key)) {
          if (key < root->data) root = root->left_child;
          else root = root->right_child;
        }
      }
      else {
        /*if (balance > 1) {
          int ghd = get_height_diff(root->left_child);
          if (ghd >= 0) {
            root = rotate_right(root);
            return root;
          }
          else {
            root = rotate_left_right(root);
            return root;
          }
        }
        if (balance < -1) {
          int ghd = get_height_diff(root->right_child);
          if (ghd <= 0) {
            root = rotate_left(root);
            return root;
          }
          else {
            root = rotate_right_left(root);
            return root;
          }
        }*/
        root = rebalance(&root);
        return root;
      }
    }
    return temp;
  }
  //AVL 트리의 탐색함수
  AVL_node* avl_search(AVL_node* node, int key)
  {
    if (node == NULL) {
      printf("Not Found!(%d)", key);
      return NULL;
    }
    printf("%d ", node->data);
    if (key == node->data) return node;
    else if (key < node->data)
      return avl_search(node->left_child, key);
    else
      return avl_search(node->right_child, key);
  }

  void main()
  {
    //7,8,9,2,1,5,3,6,4 삽입
    avl_add(&root, 7);
    avl_add(&root, 8);
    avl_add(&root, 9);
    avl_add(&root, 2);
    avl_add(&root, 1);
    avl_add(&root, 5);
    avl_add(&root, 3);
    avl_add(&root, 6);
    avl_add(&root, 4);
    avl_search(root, 9);
    printf("\n-------------------\n");

    root = avl_delete(root, 9);
    printf("search 9 : ");
    avl_search(root, 9);
    printf("\n");
    printf("search 8 : ");
    avl_search(root, 8);
    printf("\n");
    printf("search 7 : ");
    avl_search(root, 7);
    printf("\n");
    printf("search 6 : ");
    avl_search(root, 6);
    printf("\n");
    printf("search 5 : ");
    avl_search(root, 5);
    printf("\n");
    printf("search 4 : ");
    avl_search(root, 4);
    printf("\n");
    printf("search 3 : ");
    avl_search(root, 3);
    printf("\n");
    printf("search 2 : ");
    avl_search(root, 2);
    printf("\n");
    printf("search 1 : ");
    avl_search(root, 1);
    printf("\n");
  }
  ```

## Hashing
* Searching way to calculate position that key exists by using arithmetic operation
* Calculate address about key value by hash function, and move to the corresponding address in the hash table  
* Hash function => function returning the element's position about the key value
* Collision => For two different key k1,k2, if Hashtable(k1) = Hashtable(k2)

### Collision Solution
<strong>1. Open Addressing</strong>
  * Save the value with collision at other position of hash table
  * Linear probing, Quadratic probing, Double hashing, Random probing
  * Insertion, Deletion, Searching => O(1)~O(n)
  * Example : Linear Probing
    ```c
    
    #include <stdio.h>
    #include <string.h>
    #include <stdlib.h>

    #define KEY_SIZE	10	// 탐색키의 최대길이  
    #define TABLE_SIZE	13	// 해싱 테이블의 크기=소수 
    #define DEBUG

    typedef struct
    {
      char key[KEY_SIZE];
      // 다른 필요한 필드들 
    } element;

    element hash_table[TABLE_SIZE];		// 해싱 테이블 
    void init_table(element ht[])
    {
      int i;
      for (i = 0; i<TABLE_SIZE; i++) {
        ht[i].key[0] = NULL;
      }
    }
    // 문자로 된 키를 숫자로 변환
    int transform1(char *key)
    {
      int number = 0;
      while (*key)
        number = number + *key++;

    #ifdef DEBUG
      printf("transform : %d \n", number);
    #endif
      return number;
    }
    // 제산 함수를 사용한 해싱 함수
    int hash_function(char *key)
    {
      // 키를 자연수로 변환한 다음 테이블의 크기로 나누어 나머지를 반환
      return transform1(key) % TABLE_SIZE;
    }
    #define empty(item) (strlen(item.key)==0)
    #define equal(item1, item2) (!strcmp(item1.key,item2.key))

    // 선형 조사법을 이용하여 테이블에 키를 삽입하고, 
    // 테이블이 가득 찬 경우는 종료     
    void hash_lp_add(element item, element ht[])
    {
      int i, hash_value;
      hash_value = i = hash_function(item.key);
    #ifdef DEBUG
      printf("hash_address=%d\n", i);
    #endif
      while (!empty(ht[i])) {
        if (equal(item, ht[i])) {
          fprintf(stderr, "탐색키가 중복되었습니다\n");
          exit(1);
        }
        i = (i + 1) % TABLE_SIZE;
        if (i == hash_value) {
          fprintf(stderr, "테이블이 가득찼습니다\n");
          exit(1);
        }
      }
      ht[i] = item;
    }

    // 선형조사법을 이용하여 테이블에 저장된 키를 탐색
    void hash_lp_search(element item, element ht[])
    {
      int i, hash_value;
      hash_value = i = hash_function(item.key);
      while (!empty(ht[i]))
      {
        if (equal(item, ht[i])) {
          fprintf(stderr, "탐색 %s: 위치 = %d\n", item.key, i);
          return;
        }
        i = (i + 1) % TABLE_SIZE;
        if (i == hash_value) {
          fprintf(stderr, "찾는 값이 테이블에 없음\n");
          return;
        }
      }
      fprintf(stderr, "찾는 값이 테이블에 없음\n");
    }
    // 해싱 테이블의 내용을 출력
    void hash_lp_print(element ht[])
    {
      int i;
      printf("\n===============================\n");
      for (i = 0; i<TABLE_SIZE; i++)
        printf("[%d]	%s\n", i, ht[i].key);
      printf("===============================\n\n");
    }

    // 해싱 테이블을 사용한 예제 
    int main(void)
    {
      char *s[7] = { "do", "for", "if", "case", "else", "return", "function" };
      element e;

      for (int i = 0; i < 7; i++) {
        strcpy(e.key, s[i]);
        hash_lp_add(e, hash_table);
        hash_lp_print(hash_table);
      }
      for (int i = 0; i < 7; i++) {
        strcpy(e.key, s[i]);
        hash_lp_search(e, hash_table);
      }
      return 0;
    }
    ```

<strong>2. Chaining</strong>
  * Assign linked list capable of insertion/deletion to each bucket
  * Insertion => If save at linked list's head, O(1) / If save at linked list's tail, O(α)
  * Deeletion, Searching => O(α)~O(n)
  * Example
    ```c
    #include <stdio.h>
    #include <string.h>
    #include <stdlib.h>
    // 해싱 테이블의 내용을 출력
    #define TABLE_SIZE	7	// 해싱 테이블의 크기=소수 

    typedef struct {
      int key;
    } element;

    struct list
    {
      element item;
      struct list* link;
    };
    struct list* hash_table[TABLE_SIZE];

    // 제산 함수를 사용한 해싱 함수
    int hash_function(int key)
    {
      return key % TABLE_SIZE;
    }

    struct list** hashAdd(struct list** ht, struct list* node) {
      int address = hash_function(node->item.key);
      if (ht[address] == NULL) {
        ht[address] = node;
        return ht[address];
      }
      else {
        struct list* temp = ht[address];
        while (1) {
          if (ht[address]->link == NULL) {
            ht[address]->link = node;
            break;
          }
          ht[address] = ht[address]->link;
        }
        ht[address] = temp;
        return ht[address];
      }
    }
    void searchHash(int key) {
      int address = hash_function(key);
      struct list* temp = hash_table[address];
      while (1) {
        if (temp == NULL) {
          printf("탐색 %d 실패\n", key);
          break;
        }
        if (temp->item.key == key) {
          printf("탐색 %d 성공\n", key);
          break;
        }
        temp = temp->link;
      }
    }

    // 해싱 테이블을 사용한 예제 
    int main(void)
    {
      int data[9] = { 5, 8, 2, 13, 4, 6, 9, 11, 7};

      for (int i = 0; i < 9; i++) {
        struct list* node = (struct list*)malloc(sizeof(struct list));
        node->link = NULL;
        node->item.key = data[i];
        hash_table[hash_function(data[i])] = hashAdd(hash_table ,node);
      }
      printf("=======================================\n");
      for (int i = 0; i < TABLE_SIZE; i++) {
        printf("[%d]->", i);
        struct list* temp = hash_table[i];
        while (temp != NULL) {
          printf("%d->", temp->item.key);
          temp = temp->link;
        }
        printf("\n");
      }
      printf("=======================================\n");
      for (int i = 0; i < 9; i++) {
        searchHash(data[i]);
      }
    }
    
    ```


