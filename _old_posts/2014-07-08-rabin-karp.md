---
title: Rabin-Karp
layout: post
tags:
  - c
  - Rabin-Karp
  - string

---

	#include <math.h>
	#include <assert.h>  
	#include <string.h>  
	#include <stdlib.h>  
	#define d 256// number of characters in the alphabet  
	#define PRIME 127 //A prime number

	void RABIN_KARP_MATCHER( char *T, char *P, int q)
	{  
	    assert( T && P && q > 0 );  
	    int M = strlen( P );  
	    int N = strlen( T );  
	    int i, j;  
	    int p = 0;//hash value for pattern  
	    int t = 0;//hash value for txt  
	    int h = 1;  
	      
	    //the value of h would be "pow( d, M - 1 ) % q "      
	    for( i = 0; i < M - 1; i++)  
	        h = ( h * d ) % q;  
	  
	    for( i = 0; i < M; i++ )  
	    {  
	        p = ( d * p + P[i] ) % q;  
	        t = ( d * t + T[i] ) % q;  
	    }  
	      
	    //Slide the pattern over text one by one  
	    for( i = 0; i <= N - M; i++)  
	    {  
	        if( p == t)  
	        {  
	            for( j = 0; j < M; j++)  
	                if(T[i+j] != P[j])  
	                    break;  
	            if( j == M )  
	                printf("Pattern occurs with shifts: %d\n", i);  
	        }  
	        //Caluate hash value for next window of test:Remove leading digit,  
	        //add trailling digit  
	        if( i < N - M )  
	        {  
	            t = ( d * ( t - T[i] * h ) + T[i + M] ) % q;  
	            if( t < 0 )  
	                t += q;//按照书上的伪代码会出现t为负的情况，则之后的计算就失败了。  
	        }  
	    }  
	}     
	  
	int main(int argc, char* argv[])  
	{  
	    char txt[] = "GEEKS FOR GEEKS";  
	    char pat[] = "GEEK";  
	    RABIN_KARP_MATCHER( txt, pat, 127 );  
	      
	    return 0;  
	}