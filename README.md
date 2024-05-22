# Stone-Game-II

Alice and Bob continue their games with piles of stones.  There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].  The objective of the game is to end with the most stones. 
Alice and Bob take turns, with Alice starting first.  Initially, M = 1.
On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M.  Then, we set M = max(M, X).
The game continues until all the stones have been taken.
Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

Input
There are two lines of input:
The first line will have an integer as input representing the size of array piles.
The second line will have n space-separated integers representing elements of piles array.

Constraints:
1 ≤ piles.length ≤ 100
1 ≤ piles[i] ≤ 104

def stoneGameII(piles):
    n = len(piles)
    
    # Suffix sums to quickly get the total stones from i to end
    suffix_sum = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suffix_sum[i] = suffix_sum[i + 1] + piles[i]

    # Memoization table
    memo = {}

    def maxStones(i, M):
        if i == n:
            return 0
        if (i, M) in memo:
            return memo[(i, M)]
        
        max_stones = 0
        total_stones = 0
        for x in range(1, 2 * M + 1):
            if i + x > n:
                break
            total_stones = suffix_sum[i] - suffix_sum[i + x]
            max_stones = max(max_stones, total_stones + suffix_sum[i + x] - maxStones(i + x, max(M, x)))
        
        memo[(i, M)] = max_stones
        return max_stones

    return maxStones(0, 1)

if __name__ == "__main__":
    import sys
    input = sys.stdin.read
    data = input().split()
    n = int(data[0])
    piles = list(map(int, data[1:n+1]))
    print(stoneGameII(piles))
