---
layout: post
category: Git
title: algorithm
tagline: by TinyBread
tags: [algorithm]
---
<!--more-->

Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution.

Example:

        Given nums = [2, 7, 11, 15], target = 9,
        Because nums[0] + nums[1] = 2 + 7 = 9,
        return [0, 1].
        
        
Solution: JAVA
(배열안의 값은 정렬되어 있다고 가정)

    - Sol 1 binary search를 활용
    
    public class Solution {
        public int[] twoSum(int[] nums, int target) {
            
            int resultIndex[2] = {0}
            int index=-1;
            for(int i=0;i<nums.length;i++) {
                
                resultIndex[0] = target-nums[i];
                
                //이진탐색 알고리즘으로 해당하는 index값 을 반환
                index=binarySearch(compareNum);
                
                if(binarySearch(compareNum)!=-1) {
                     resultIndex[1]=index;
                     return resultIndex;
                } 
            }
        throw new IllegalArgumentException("No two sum solution");
        }
    }




    
    - Sol 2 HashMap을 이용
    <br> HashMap의 containsKey와 List의 contains는 해당 값을 search하는 방식이 다르다.
    <br> List에서는 순차적으로 해당하는 값을 찾는반면 HashMap의 경우 Java 8에서는 데이터의 개수가 많아지면, Separate Chaining에서 링크드 리스트 대신 트리를 사용하기 때문에 성능상의 이점이 있다.
(하나의 해시 버킷에 8개의 키-값 쌍이 모이면 링크드 리스트를 트리로 변경한다)

        
        public int[] twoSum(int[] nums, int target) {
            Map<Integer, Integer> map = new HashMap<>();
          
            for (int i = 0; i < nums.length; i++) {
                int complement = target - nums[i];
                if (map.containsKey(complement)) {
                    return new int[] { map.get(complement), i };
                }
                map.put(nums[i], i);
            }
            throw new IllegalArgumentException("No two sum solution");
        }
    
    
    
### 참조
- [https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)