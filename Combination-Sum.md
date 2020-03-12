# LeetCode: Combination-Sum 
Link to the original problem: https://leetcode.com/problems/combination-sum/
## Intuition 
The problem expects us to get a list of lists such that the integers in each list sum up to a given target. 
For this, firstly we will sort our candiates array in ascending order. 
We do this as it tells when to stop if target construction has been reached or can no longer be achieved with remaining integers as they are bigger. 
Then, we need to realise that at any given point while constructing a combination, we can do one of the 2 things. 
We can either include an integer(1+ times) or not include an integer.

What that means is -
1. If we include an integer: 
we subtract it from the target, target gets reduced and that integer gets added in a list of potential combination. It can be considered again.
2. If we do not include an integer: 
we go to the next number in the array and our target does not change. 

So we write a function that does this and call it recursively till our target becomes 0. 
When our target reduces to 0, we get the list of numbers and store that list in a resultant list as there could be more than one such combination.
We return the resultant list as solution.

## Code
```
class Solution {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    int candidates[];
    
    public List<List<Integer>> combinationSum(int[] candidatess, int target) {
        candidates = candidatess; //Giving it global access
        Arrays.sort(candidates); 
        ArrayList<Integer> arr = new ArrayList<Integer>();
        f(arr,target, 0); //Initial call with index 0
       return result;
    }
    
    public void f(ArrayList<Integer> arr, int target, int pos) {
        if(target == 0) {
            if(!arr.isEmpty()) {
                result.add(new ArrayList<>(arr));
            }
            return;
        }
        // Target is not fully achieved but 'pos' has reached end of array so no more combination can be made.
        if(pos == candidates.length) return; 
        
        // As array is sorted and 'pos' is updated sequentially, if this condition is true no further integer can be used to build a combination.
        if(candidates[pos] > target) return; 
        
        f(arr, target, pos+1); 
        
        // Adding element to end of array as that is included in combination.
        arr.add(candidates[pos]);
        
        f(arr, target-candidates[pos], pos);
        
        // Undoing the add by removing the last element, to leave array in correct state.
        arr.remove(arr.size()-1);
    }
}
```

## Time Complexity and Space Complexity Analysis

#### Time Complexity:

It takes O(nlogn) to sort the candidate array; n is the length of array.

O(S,pos) = O(S,pos+1) + O(S-arr[pos],pos) ; S is the target sum and pos is the position in candidates array.

So the time complexity is O(2^S) + O(nlogn) which is still O(2^S) for the recursion of finding combination of all integers that sum to S.

#### Space Complexity:

We construct a new array list only when we get a right combination and are resusing an array list for every other potential combination. 

We are creating 2^S different combinations for worst case scenario, and hence space complexity is O(2^S).
