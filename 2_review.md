# 복습2

## 1. 같은 숫자는 싫어(stack & queue)

문제 : https://school.programmers.co.kr/learn/courses/30/lessons/12906

- 내 풀이

'''c++
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
'''
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
 
