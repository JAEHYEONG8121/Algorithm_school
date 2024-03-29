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
- 하지만, 위의 코드에서 또 하나의 핵심이 존재한다.
	- 만약 참가자 명단이 a, b, c이고, 완주자 명단이 a, b이면 for문을 완주자 벡터의 사이즈로 돌리기 때문에 c가 완주하지 못한 사람으로 추출되지 않을 수 있다.
	- 이와 같은 예외처리를 participant[i]로 처리한 것이다. i를 따로 변수로 선언했으므로 위와 같은 경우 for문을 돌리고 나면 i = 2가 되어있고, 어쨋든 참가자 명단에는 완주하지 못한 사람이 무조건 한명이 있기 때문에 for문이 break되지 않고 그냥 흘러지나갔다면 참가자 명단의 마지막 인덱스를 이용하여 결과를 출력하면 완주하지 못한 사람을 출력할 수 있는 것이다!!!


**다른 풀이**

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>

string solution2(vector<string> participant, vector<string> completion)
{
	string answer = "";
	unordered_map<string, int> map;
	
	for (auto player : participant)
	{
		if (map.end() == map.find(player))
			map.insert(make_pair(player, 1));
		else
			map[player]++;;
	}

	for (auto player : completion)
		map[player]--;

	for (auto player : participant)
		if (map[player] > 0)
		{
			answer = player;
			break;
		}

	return answer;
}
```

- 위의 코드에서는 HashMap을 만들었다. HashMap이란 Key-Value의 Pair를 관리하는 자료구조이다!
- unordered_map<string, int> map으로 지정하면 Key는 string, Value는 integer형태로 정의하는 것이다.
	- 이 문제에서는 Key는 participant의 이름이 되고, Value는 cnt의 의미로 integer형태로 선언해준 것이다.
	
- Hashing하기
	- HashMap에 participant를 전부 추가해준다.
	- map.insert(make_pair(player, 1)) : HashMap에 Key와 Value를 한 쌍으로 입력하는 함수이다.
	- insert가 되어있는 player에 대해서는 map[player]++ 혹은 map[player]--로 쉽게 연산할 수 있다.

- HashMap을 만들어서 각 참가자들의 cnt를 1로 설정해둔다.
- 그리고 completion명단을 훑어서 완주한 사람의 경우 -1을 해줌으로써 완주한 player의 map의 value가 0이 되도록 한다. -> 결과적으로 value값이 그대로 1인 경우 그 사람이 완주하지 못한 유일한 한명이 되는 것이다!

- unordered_map은 HashTable로 구현한 자료구조로 탐색의 시간복잡도는 O(1)이다. 이는 Binary Search Tree인 map의 시간복잡도인 O(logn)보다도 월등히 좋은 성능을 보이는 것을 알 수 있다.
- 하지만, Key가 유사한 데이터가 많을 시 해시 충돌로 인해 성능이 떨어질 수도 있다.

***

## 3. 전화번호 목록(hash)

- 문제 : https://school.programmers.co.kr/learn/courses/30/lessons/42577

**내 풀이**

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

bool solution(vector<string> phone_book)
{
	sort(phone_book.begin(), phone_book.end());

	for (int i = 0; i < phone_book.size()-1; i++)
	{
		if (phone_book[i] == phone_book[i + 1].substr(0, phone_book[i].size()))
		{
			return false;
		}
	}

	return true;
}
```

- 내 풀이의 핵심은 바로 정렬이다!
	- string이 담겨있는 phone_book 벡터를 정렬하면 사전순으로 정렬이 된다!
	- 즉, ["119", "97674223", "1195524421"] 이와 같은 경우, 119, 1195524421, ...순으로 정렬이 된다.

- 그렇다면 이렇게 정렬된 상태에서는 벡터의 원소 앞뒤로만 비교를 해도 된다는 것이다!
	- 정렬된 값이므로 인덱스 i와 i+1이 접두어가 존재하는 관계가 아니라면 i와 i+2는 당연히 그런 관계를 맺을 수 없다!
	- 그리고, 비교를 할때는 앞의 원소가 사전순으로 앞서고 상대적으로 문자열의 길이도 짧을 것이므로 i+1의 원소를 substr()메서드를 이용하여 짤라서 비교해주면 된다!
	- 비교해서 같은 경우 바로 return false를 해주면 된다!

**다른 풀이**

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>

unordered_map<string, int> map;

	for (int i = 0; i < phone_book.size(); i++)
		map[phone_book[i]] = 1;

	for (int i = 0; i < phone_book.size(); i++)
	{
		string phone_number = "";
		for (int j = 0; j < phone_book[i].size(); j++)
		{
			phone_number += phone_book[i][j];
			if (map[phone_number] && phone_number != phone_book[i])
				return false;
		}
	}
	return true;
```

- 위의 풀이 역시 Hash Map을 이용했다.
- map을 만들어서 phone_book의 원소들을 넣어주었다.
- phone_number라는 빈 문자열을 하나 만들고, for문을 돌리면서 한 글자씩 더해갔다.
	- phone_number에 문자가 하나씩 더해지면서 map에 존재하는 문자열이 된 상태에서, phone_number가 그 당시의 for문의 인덱스에 해당하는 phone_book의 원소와 다르다면 한 번호가 다른 번호의 접두사인 경우가 있다는 의미이므로 false를 return해준것이다!

***

## 4. 가장 큰 수(sort)

- 문제 : https://school.programmers.co.kr/learn/courses/30/lessons/42746

**내 풀이**

```c++


```







