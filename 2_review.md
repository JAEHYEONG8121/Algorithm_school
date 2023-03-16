# 복습2

## 1. 같은 숫자는 싫어(stack & queue)

문제 : https://school.programmers.co.kr/learn/courses/30/lessons/12906

- 내 풀이

''''
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
''''
 
