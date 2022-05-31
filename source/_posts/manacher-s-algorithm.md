title: manacher's algorithm
date: 2016-05-11 13:32:50
categories: 
- algorithm and data structure
tags: palindrom
---

# manacher's algorithm

## what's it?
manacher's algorithm is good at calculate palindrome string, the time-complexity can reach o(n). it would re-use the pattern to calcuate, so it is very efficient.

## code
```
package com.weixin;

public class ManachersAlgorithm {
    
    public  int findLongestPalindrome(String s) {
        if(s==null || s.length() <= 1){
            return s.length();
        }
        
        int[] p = new int[s.length()];
        p[0] = 1;
        int idx = 0, rMax = 0;
//        S  #  1  #  2  #  2  #  1  #  2  #  3  #  2  #  1  #
//        P  1  2  1  2  5  2  1  4  1  2  1  6  1  2  1  2  1
        for(int i = 1; i < s.length(); i++){
             /** --left------j------idx------i--------rMax-----
             	*idx means the center which has the maximum radius of palindrome , include himself
             	*p[i] means the radius of the palindrome center on i, include i itself
             	*so rMax = idx + p[idx] - 1 ( since include idx itself, so we need to minus 1)
             	*for example when i is 7, then p[7] = 4, rMax = 4+7 - 1 = 10,  1 # 2 #(rMax is here)
             	*i is the current index, j is the index which is symetric pointer centered on idx
             	*since i - idx = idx -j , then we get j = 2 * idx - i, but the pre-requsite is 2*idx -i >= 0
             	*if rMax - i > p[j], what does that mean? 
             	*firstly, we already know p[j], secondly we know p[idx] and rMax, let's give an example
             	*|------|----j---|----^-----i------|------|
             	*|------l----j===p----idx-------i===p'---r------|
             	*assume p[j] is at the position p, i is symetric, assume p[i] is at position p', since i and j are symetric,
             	*we know dist(j,p) = p[j] == dist(i,p'), if p' is between i and rMax,
             	*which means p[j] is totally contained in the long palindrome, and we can directly know
             	*that p[i] = p[j], then we don't need to calculate any more.
             	*for ex,
             	* 
             	* S  #  1  #  2  #  2  #  1  #  2  #  3  #  2  #  1  #
             	* P  1  2  1  2  5  2  1  4  1  2  1  6  1  2  1  2  1
             	* ------------j--^--i--------r----
             	* we know p[j] is 2, p[idx]=5, rMax = idx+p[idx]-1 = 4+5-1 = 8, j = 3, idx=4, i = 5
             	* rMax - i = 8-5 = 3 > p[j] = 2, so we directly know p[5] = p[3] = 2
             	* 
             	*another case is j + p[j] > idx, what it looks like : 
             	*|------|----j------^-----|-----i------|-----|
             	*|------l----j------idx---p-----i------r-----|
             	*in this case, rMax -i < p[2*idx-i], then we can not directly get p[i], but we still can re-use p[j]
             	*we already know centered on i, all the way to rMax it is palindrome, so we just need to calculate start from rMax
             	*this also save some calculation
           
             	*
             **/
            
            if( 2 * idx >= i && rMax - i > p[2*idx - i]){
                p[i] = p[2*idx-i];
            }
            else{
               /**
                * if i is already reach or over the rMax boader, we need to re-calcuate
                * at below, we need to re-use p[rMax-i], so we need to gurantee rMax-i >= 0
                */
               if(i >= rMax){
                   idx = i;
                   p[i] = getPalLen(s, i);
                   rMax = i + p[i]-1;
               }
               else{
                   /**
                    * as we explained above, if p[j] is so big, which make the rMax - i < p[j(j=2*idx-i)]
                    * in this case, at least we know start from i to rMax, it must be a palindrome, and we can re-use this result
                    * so we have p[i] = p[rMax -i]
                    */
                   p[i] = p[rMax -i];
                   /**start from i+p[i], we check palindrome,go left and right, compare one by one, then we increase p[i] accordingly
                    *  but we need to be aware that i+p[i] < s.length, I made a mistakes here, debug it for quite some time.
                    *  
                    */ 
                   while(i+p[i] < s.length() && s.charAt(i+p[i])==s.charAt(i-p[i])){
                       p[i]++;
                       
                   }
                   /**
                    * if i+p[i] larger than rMax already, then update rMax
                    */
                   if(i+p[i] > rMax){
                       idx = i;
                       rMax = i + p[i]-1;
                   }
               }
            }
            
        }
        
        int k = 0;
        for(int a : p){
            k = Math.max(k,a);
        }
        return k -1;
    }
    
    public int getPalLen(String s, int i){
        int l = i, r = i;
        while(l>=0 && r <= s.length()-1){
            if(s.charAt(l)!=s.charAt(r)){
                break;
            }
            l--;
            r++;
        }
        return r-i;
    }
    
    public String specialHandler(String s){
        StringBuilder sb  = new StringBuilder();
        sb.append('#');
        for(char c : s.toCharArray()){
            sb.append(c);
            sb.append('#');
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        ManachersAlgorithm ma = new ManachersAlgorithm();
        String s = "12212321";
        String s1 = ma.specialHandler(s);
        int k = ma.findLongestPalindrome(s1);
        System.out.println(k);
        s = "abaxabaxabb";
        s1 = ma.specialHandler(s);
        k = ma.findLongestPalindrome(s1);
        System.out.println(k);
        s = "abaxabaxabybaxabyb";
        s1 = ma.specialHandler(s);
        k = ma.findLongestPalindrome(s1);
        System.out.println(k);
        
    }

}
```

## result



