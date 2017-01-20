# test2
just test
/********************************************************************************************
 **
 ** File:    MinValueHeap.cpp
 ** Project: CMSC 341 Project 4, Fall 2016
 ** Author:  Tatiana Nikolaeva Section 01
 ** Date:    11/20/2016
 ** E-mail:  bn22573@umbc.edu
 **
 ** MinValueHeap.cpp contains imlementations of  constructor, destructor,variables, and functions
 ** for MinValueHeap.
 **
 *********************************************************************************************/
#include "MinValueHeap.h"
#include <iostream>

//-------------------------------------------------------------------------------------------
//                               destructor
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
MinValueHeap<T, m_size>::~MinValueHeap()
{
}
//-------------------------------------------------------------------------------------------
//                               default constructor
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
MinValueHeap<T, m_size>::MinValueHeap()
:Heap<T,m_size>::Heap()
{
}

//-------------------------------------------------------------------------------------------
//                               copy constructor
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
MinValueHeap<T, m_size>::MinValueHeap(Heap <T, m_size>&origHeap)
:Heap<T,m_size>::Heap(origHeap)
{
   //copy given heap into host heap
   for (int i = 0; i < origHeap.real_size;  i++)
   {
      this->heapArray[i] = origHeap.heapArray[i];
   }
   this->m_type = origHeap.m_type;         // save type
   this->real_size = origHeap.real_size;   // save size
   Heapify_min(origHeap.real_size);
}
//-------------------------------------------------------------------------------------------
//                               heapify
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
void MinValueHeap<T, m_size>::Heapify_min(int size)
{
    int length = size;
    for(int i=length-1; i>=0; --i)
    {
        PercolateDown_min(i, length);
    }
}
//-------------------------------------------------------------------------------------------
//                               percolate down
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
void MinValueHeap<T, m_size>::PercolateDown_min(int index, int size)
{
    
    int length = size ;
    int leftChildIndex = 2*index + 1;
    int rightChildIndex = 2*index + 2;
    if(leftChildIndex >= length)
        return; //index is a leaf

    int minIndex = index;
   //compare values and assign min index
    if(this->heapArray[index].GetValue() > this->heapArray[leftChildIndex].GetValue())
    {
        minIndex = leftChildIndex;
    }
    //compare values and assign min index
    if((rightChildIndex < length) && (this->heapArray[minIndex].GetValue() > this->heapArray[rightChildIndex].GetValue()))
    {
        minIndex = rightChildIndex;
    }
    if(minIndex != index)
    {
        //need to swap
        T temp(this->heapArray[index].GetKey(),this->heapArray[index].GetValue());
        this->heapArray[index] = this->heapArray[minIndex];
        this->heapArray[minIndex] = temp;
        PercolateDown_min(minIndex,size);
    }
}
//-------------------------------------------------------------------------------------------
//                               percolate up
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
void MinValueHeap<T, m_size>::PercolateUp_min(int index)
{
    if(index == 0)
        return;

    int parentIndex = (index-1)/2;

    if(this->heapArray[parentIndex].GetValue() > this->heapArray[index].GetValue())
    {
        T temp(this->heapArray[parentIndex].GetValue(),this->heapArray[parentIndex].GetValue());
        this->heapArray[parentIndex] = this->heapArray[index];
        this->heapArray[index] = temp;
        PercolateUp_min(parentIndex);
    }
}
//-------------------------------------------------------------------------------------------
//                               insert
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
void MinValueHeap<T, m_size>::Insert_min(T& insertable)
{
    int length = this->heapArray.size();
    this->heapArray[length] = insertable;

    PercolateUp_min(length);
}
//-------------------------------------------------------------------------------------------
//                               get min
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
T& MinValueHeap<T, m_size>::GetMin()
{
    return this->heapArray[0];
}
//-------------------------------------------------------------------------------------------
//                               delete min
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
T& MinValueHeap<T, m_size>::DeleteMin(int size)
{
    int length = size;
    hit = this->heapArray[0];
    this->heapArray[0] = this->heapArray[length-1]; //swap
    
   PercolateDown_min(0,length);

    return hit;
}

//-------------------------------------------------------------------------------------------
//                               find min
//-------------------------------------------------------------------------------------------
template<class T, int m_size>
const T* MinValueHeap<T, m_size>::Find_min( const T& needle) const
{
   for (int i = 0; i < 10000; i++)
   {
       if (this->heapArray[i].GetKey() == needle.GetKey())
           return &this->heapArray[i];  // if found
   }
   return NULL;
}
