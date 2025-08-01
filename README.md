
#  Transposed Direct Form Ⅱ


To understand the concept of the implementation structure, let's visit the generic formula for the transfer function of the 2nd-order IIR filter

<p align="center">
  <img src="https://github.com/user-attachments/assets/58acc3b2-5119-4c98-b5a3-7bfb157a2796" width="239" height="71" alt="image" />
</p>

<br>

### Implementation Structure : Transposed Direct Form Ⅱ

<p align="center">
<img width="586" height="424" alt="image" src="https://github.com/user-attachments/assets/e17e8642-7437-4ce1-86ee-337d97f888d8" />
</p>

In the case of transposed direct form 2, the difference equation is 

<p align="center">
<img width="283" height="47" alt="image" src="https://github.com/user-attachments/assets/b11faa9b-4e5c-4f56-b421-244e5d6f7b61" />
</p>

<p align="center">
<img width="283" height="86" alt="image" src="https://github.com/user-attachments/assets/00510b61-c9b1-4c09-b054-9742ae16ca90" />
</p>

"+1" in the buffers' arguments indicates that they will be used in the next iteration

<br>

The transposed direct form II structure should be the default choice for implementing second-order IIR filters.
However, other structures such as second-order sections or ladder structures [Smith2007] also exist.
These structures can be useful in specific situations, but for most general applications, transposed direct form II is sufficient.

<br>

### Implementation

~~~cpp
float s1 = 0.0f;
float s2 = 0.0f;

float TransposedDirectForm2(float x)
{
    y = x * b0 + s1;
    
    float s1_next = x * b1 + s2 - a1* y;
    float s2_next = x * b2 - a2 * y;
    
    s1 = s1_next;
    s2 = s2_next;
    
    return y;
}
~~~

Before any processing begins, we initialize the internal state buffers `s1` and `s2` to zero.

The filter processes a single input sample `x` and returns the corresponding output sample `y`.

For each call to the function, 
the output `y` is first computed using the current input `x`, the feedforward coefficient `b0`, and the previous buffer `s1`.

Once the output is calculated, the internal buffers are updated using the current input `x`, 
the computed output `y`, and the remaining filter coefficients `b1`, `b2`, `a1`, and `a2`.

The buffer update equations ensure that the internal state reflects the latest input-output relationship, maintaining the recursive behavior of the IIR filter.

This structure follows the transposed direct form II, which minimizes memory usage and is known for its numerical stability.



