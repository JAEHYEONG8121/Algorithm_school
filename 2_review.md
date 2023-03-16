# 복습2

## 1. 같은 숫자는 싫어(stack & queue)

문제 : https://school.programmers.co.kr/learn/courses/30/lessons/12906

**내 풀이**

```c++
#include <iostream>
#include <vector>

using namespace std;

vector<int> solution(vector<int> arr)
{
	vector<int> answer;

	for (int i = 0; i < arr.size(); i++) {
		if (i == arr.size()-1 || arr[i] != arr[i + 1])
			answer.push_back(arr[i]);
	}
	return answer;
}
```

- 연속적으로 나오는 수는 하나만 남기고 전부 제거해야한다.
    - 1. 벡터의 원소 제거 방법
    - 2. 새로운 벡터를 생성하여 원소를 넣어주는 방법

- 처음에는 1번의 방법을 생각했다. 하지만 원소 제거는 시간복잡도상 효율적이지 않을 수 있다고 판단하여, 2번으로 풀어보았다.
- 입력으로 들어온 벡터를 순회하며 앞과 뒤를 비교하여 서로 다르다면 앞의 원소부터 새로 생성한 벡터 answer에 삽입한다.
- 하지만 이렇게 하면 벡터의 마지막 원소처리에 문제가 발생한다.
    - 마지막 두 원소가 연속된 1 1 이면 1을 출력해야하지만, 앞과 뒤를 비교하여 순회하면서 다르면 앞의 원소를 추가한다면 이와 같은 상황에서는 아무 원소도 추가되지 못한다.
    - 그러므로 조건문에 맨 마지막 원소의 차례일 때 무조건 push해주는 조건을 or로 처리하면 이와 같은 문제가 해결 가능하다
    - if) 1 1과 같이 연속된 경우가 마지막에 오는 경우 -> 첫번째 1은 if문의 뒤의 조건으로 인하여 push되지 않는다 but, 마지막 원소 차례가 오면 1이 push되므로 해결
    - if) 1 2와 같이 연속되지 않은 수가 마지막에 오는 경우 -> 첫번째 1은 if문의 뒤의 조건으로 인하여 서로 다르기 때문에 push됨 + 마지막 원소 2는 if문의 앞 조건에 의해 push되므로 해결



**다른 풀이 v1**

```c++
#include <vector>
#include <iostream>

using namespace std;

vector<int> solution(vector<int> arr) 
{
    vector<int> answer;
    
    for(int i = 0; i < arr.size(); i++)
    {
        if(answer.size() == 0 || answer[answer.size() - 1] != arr[i]) 
            answer.push_back(arr[i]);
    }

    return answer;
}
```

- if조건문 한줄만 다른 코드이다.
	- 앞의 조건을 보면, answer벡터에 원소가 없을 때는 일단 입력으로 들어온 벡터 arr의 첫번째 원소를 삽입해준다.
	- 그 후로는 answer에 들어있는 가장 오른쪽 원소와 입력으로 들어온 arr의 원소와 비교하여 다르다면 arr원소를 answer에 계속 삽입해나가는 방식이다.
- 거의 같은 코드이지만 위 코드는 마지막 원소의 처리를 따로 해줄 필요없이 일관성있게 arr원소와 answer의 원소를 비교해가며 연속된 수를 제거하고 하나만 남길 수 있게 된 것이다.



**다른 풀이 v2**

```c++
#include <vector>
#include <iostream>
#include <algorithm>

using namespace std;

vector<int> solution(vector<int> arr) 
{    
    arr.erase(unique(arr.begin(), arr.end()), arr.end());
    vector<int> answer = arr;

    return answer;
}
```

- erase()와 unique()를 이용하여 간단하게 짠 코드이다
- unique()는 원소 갯수 n에 대하여 O(n)의 시간복잡도를 갖기에 아주 효율적인 방법이다.
- 자세한 코드 설명은 링크를 통해 알 수 있다.
	- 출처 : https://sangwoo0727.github.io/c++/Cplus-unique/

***

## 2. 완주하지 못한 선수(hash)

- 문제 : https://school.programmers.co.kr/learn/courses/30/lessons/42576

**내 풀이**

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

string solution(vector<string> participant, vector<string> completion)
{
	sort(participant.begin(), participant.end());
	sort(completion.begin(), completion.end());

	int i = 0;
	for (i; i < completion.size(); i++)
	{
		if (participant[i] != completion[i])
			break;
	}

	return participant[i];
}
```

- 참가자 벡터와 완주자 벡터의 차이는 1이라는 것이 문제의 조건이다.
- 일단 문자열이 담긴 두 벡터를 정렬하면 알파벳 순으로 이름이 정렬될 것이다.
	- 정렬되면 같은 이름이 paticipant에서 한 명의 이름 빼고 모두 completion벡터에 존재할 것이다.
	- for문을 completion의 size만큼 돌리고 만약 이름이 다른 경우가 나오면 break하고 그 때의 participant에 있는 이름을 return해주면 된다.
- 결국 두 벡터를 정렬하는 것이 핵심이라고 할 수 있다!!





