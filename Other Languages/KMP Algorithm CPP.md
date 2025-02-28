#Brute force approach: 

#include<bits/stdc++.h>
 
using namespace std;
int kmpBrute(string String, string pattern) {
  int n = String.size(), m = pattern.size();
  for (int i = 0; i < n + 1 - m; i++) {
    int j = 0;
    while (j < m && String[i + j] == pattern[j])
      j++;
    if (j == m) return i;
  }
  return -1;
}
int main() {
  string pattern="aaaaaab", String="aaaaaaaamaaaaaab";

  int index = kmpBrute(String, pattern);
  if (index == -1) cout << "The pattern is not found";
  else cout << "The pattern " << pattern << " is found in the given string " 
  << String << " at " << index;
  return 0;
}
Output: The pattern aaaaaab is found in the given string aaaaaaaamaaaaaab at 9

Time Complexity: O(mn) where m and n are patterns and string lengths respectively.

Since at each index, all m index values are to be compared in this approach, in total we will have m*n comparisons and hence O(mn).

Space Complexity: O(1) as no space is made use of. 

#Optimal Code:

#include<bits/stdc++.h>
 
using namespace std;
int kmp(string String, string pattern) {
  int i = 0, j = 0, m = pattern.length(), n = String.length();
  pattern = ' ' + pattern; //just shifting the pattern indices by 1
  vector < int > piTable(m + 1, 0);
  for (int i = 2; i <= m; i++) {
    while (j <= m && pattern[j + 1] == pattern[i])
      piTable[i++] = ++j;
    j = 0;
  }
  j = 0;
  for (int i = 0; i < n; i++) {
    if (pattern[j + 1] != String[i]) {
      while (j != 0 && pattern[j + 1] != String[i])
        j = piTable[j];
    }
    j++;
    if (j == m) return i - m + 1;
  }
  return -1;

}
int main() {
  string pattern="aaaaaab", String="aaaaaaaamaaaaaab";

  int index = kmp(String, pattern);
  if (index == -1) cout << "The pattern is not found";
  else cout << "The pattern " << pattern << " is found in the given string " 
  << String << " at " << index;
  return 0;
}
Output: The pattern aaaaaab is found in the given string aaaaaaaamaaaaaab at 9

Time Complexity: O(m+n) where m and n are patterns and string lengths respectively.

O(m) is for creating the pi table for the pattern and O(n) is the TC of traversing the given string.

Space Complexity: For the pi table, we need a space m+1. So SC is O(m) here.
