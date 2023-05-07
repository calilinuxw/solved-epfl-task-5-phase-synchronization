Download Link: https://assignmentchef.com/product/solved-epfl-task-5-phase-synchronization
<br>
In Section 3.1.1 we said that the real part of the baseband signal is modulated onto a cosine wave at radio frequency, and the imaginary part onto a sine wave. Since the cosine and sine functions are orthogonal, the two signal parts can be separated in the receiver. However, we run into a similar problem as the one discussed in the last lab: The receiver merely sees an oscillating waveform (a sinusoid): <em>r</em>(<em>t</em>) ∝ sin(2<em>πf<sub>c</sub>t </em>+ <em>θ</em>) during one symbol duration. Is it a sine, or a cosine, or something in between? In order to differentiate between the two base functions, the receiver needs a phase reference, i.e. it needs to know the phase offset <em>θ</em>. The task of the phase synchronization unit is to provide this reference.

<h1>            5.1    Signal Model</h1>

The phase synchronization is performed after the interpolator and thus works on the timingcorrected symbols

<em>z<sub>ε</sub></em>[<em>n</em>] = <em>z</em>(<em>nT </em>+ <em>εT</em>ˆ )<em>.                                      </em>(5.1)

We will assume a perfect timing recovery and interpolation, so that <em>z<sub>ε</sub></em>[<em>n</em>] = <em>z</em>(<em>nT </em>+ <em>εT</em>).

In the complex baseband, the phase offset <em>θ </em>translates to a multiplication of the received signal with <em>e<sup>jθ</sup></em>. The effect of this is a rotation of the signal space, as illustrated in Figure 5.1. The left plot shows a sequence of received QPSK symbols after timing correction without any phase offset, with an SNR of 8 dB. The borders of the decision regions are the <em>x</em>– and <em>y</em>-axis; for example, the symbol demapper decides that the bits <em>b</em><sub>0</sub><em>b</em><sub>1 </sub>= 11 have been transmitted if the received symbol lies in the upper right quadrant of the complex plane (see Figure 1.4). In Figure 5.1a we can easily recognized the transmitted QPSK constellation, and we see that most symbols will be correctly detected, even though the clouds extend to the coordinate axes and therefore a few bit errors will occur.

Figure 5.1b shows the same sequence with a phase error of 20<sup>◦</sup>. It is obvious that the bit error rate will be far worse if we attempt to demap these symbols without correcting the phase offset.

The phase offset is not constant but varies slowly over time, e.g. due to nonideal analog components. Since the variation of the phase is a random process, this effect is called <em>phase noise</em>. The received symbol sequence can now be written as

<em>z<sub>ε</sub></em>[<em>n</em>] = <em>a</em>[<em>n</em>]<em>e<sup>jθ</sup></em><sup>[<em>n</em>] </sup>+ <em>w</em><sup>′</sup>[<em>n</em>]<em>.                                 </em>(5.2)

A simple but realistic phase noise model is the random walk: The first phase offset <em>θ</em>[0] is uniformly distributed in [0;2<em>π</em>). Then, over time, the phase offset evolves according to

<em>θ</em>[<em>n</em>] = <em>θ</em>[<em>n </em>− 1] + ∆<em>θ</em>[<em>n</em>]      (mod 2<em>π</em>)                    (5.3)

32                                                                                  Chapter5: PhaseSynchronization

−2                 −1                   0                   1                   2                                −2                 −1                   0                   1               2

Re{<em>z<sub>ε</sub></em>[<em>n</em>]}                                                                             Re{<em>z<sub>ε</sub></em>[<em>n</em>]}

(a) <em><sup>θ</sup></em>=0<sup>◦                                                                                                                                                         </sup>(b) <em><sup>θ</sup></em>=20<sup>◦</sup>

Figure 5.1: Received symbols without (a) and with (b) phase offset

Figure 5.2: Typical phase noise process for <em>σ</em><sub>∆<em>θ </em></sub>= 0<em>.</em>004

where the difference between two successive phase offsets is a Gaussian random variable

<em>.                                                 </em>(5.4)

Figure 5.2 illustrates a typical phase noise process for a standard deviation of <em>σ</em><sub>∆<em>θ </em></sub>= 0<em>.</em>004 ≈ 0<em>.</em>23<sup>◦</sup>. We see that the phase drift is rather slow, but still nonnegligible if the transmission is longer than about thousand symbols. Therefore, the phase offset must be tracked during the transmission.

5.2: SynchronizationviaRotation

Figure 5.3: Phase synchronization unit

<h1>            5.2     Synchronization via Rotation</h1>

The correction of the phase offset is straightforward. Since the phase offset causes a rotation of the signal space, <em>z<sub>ε</sub></em>[<em>n</em>] = <em>a</em>[<em>n</em>]<em>e<sup>jθ</sup></em><sup>[<em>n</em>] </sup>+ <em>w</em><sup>′</sup>[<em>n</em>], we simply rotate the signal space into the opposite direction:

<em>z</em><em>ε</em>;<em>θ</em>[<em>n</em>] = <em>z</em><em>ε</em>[<em>n</em>]<em>e</em>−<em>jθ</em>ˆ[<em>n</em>]

(5.5)

= <em>a</em>[<em>n</em>]<em>e</em><em>j</em>(<em>θ</em>[<em>n</em>]−<em>θ</em>ˆ[<em>n</em>]) + <em>w</em>′′[<em>n</em>]

with <em>w</em><sup>′′</sup>[<em>n</em>] = <em>w</em><sup>′</sup>[<em>n</em>]<em>e</em><sup>−<em>jθ</em>ˆ[<em>n</em>]</sup>. The phase synchronization unit is illustrated in Figure 5.3.

<h1>            5.3     The Estimation Algorithm</h1>

The estimation algorithm, which is known in the literature as the “Viterbi and Viterbi” algorithm [4], uses the fact that we have a QPSK modulation. We therefore know that

(5.6)

for all transmitted symbols <em>a</em>[<em>n</em>], and thus

<em>.                      </em>(5.7)

The operation (·)<sup>4 </sup>removes the information from the data symbols. Exploiting this fact, we can derive the following estimator, which operates on a symbol-by-symbol basis (i.e. does not take any correlation between the phases into account):

<em>.                                  </em>(5.8)

This estimator is approximately unbiased if the SNR is high:

arg        (5.9)

(=b) <u>1</u><sub>4 </sub>arg{<em>e</em><em>j</em>4<em>θ</em>[<em>n</em>]}

=(c) <em>θ</em>[<em>n</em>]<em>.</em>

34                                                                                  Chapter5: PhaseSynchronization

The approximation (a) is valid for a high SNR, because then the variation of the argument is small, and the nonlinear argument operation can be linearized. In this case, the expectation and the argument operation can be exchanged. Equation (b) holds because the noise has zero-mean and is uncorrelated with the data sequence.

The step (c) is does not allow us to uniquely identify any <em>θ </em>∈ [0;2<em>π</em>). To see this, consider, for example, and (we drop the symbol index for simplicity). Then, the estimator will give us:

<table width="443">

 <tbody>

  <tr>

   <td width="405"><em>θ</em><sup>ˆ</sup><sub>1 </sub>=  arg{<em>e<sup>j</sup></em><sup>4<em>θ</em></sup><sup>1</sup>} =  arg4</td>

   <td width="39">(5.10)</td>

  </tr>

  <tr>

   <td width="405"></td>

   <td width="39">(5.11)</td>

  </tr>

 </tbody>

</table>

<h1>                                                              <sup>1                             1                          <em>π</em></sup></h1>

We can only identify <em>θ </em>correctly if we know which quadrant it is in. One way to have access to this information is to consider the phase estimate for the previous symbol. Assuming that the phase changes slowly, the two consecutive phases will most likely be close to each other (BUT: not necessarily in the same quadrant). However, in order to use this method we need a initial phase information about which quadrant we can start from.

<h2>5.4     Initial Phase Estimation</h2>

An initial phase estimate can be obtained by exploiting the preamble that is used for frame synchronization in the following way. Assume that there is no noise, so that <em>z</em>[<em>n</em>] = <em>a</em>[<em>n</em>]<em>e<sup>jθ</sup></em><sup>[<em>n</em>]</sup>, and let <em>p</em>[<em>n</em>] denote the preamble sequence of length <em>N<sub>p</sub></em>. The frame synchronization function computes the following correlation:

<em>N<sub>p</sub></em>

<em>c</em>[<em>n</em>] = X<em>p</em><sup>∗</sup>[<em>i</em>]<em>z</em>[<em>n </em>− <em>i</em>]<em>,                                              </em>(5.12)

<em>i</em>=0

where <em>p</em>[<em>i</em>] = <em>p</em>[<em>N<sub>p </sub></em>− 1 − <em>i</em>] is the reverted preamble sequence. <em>c</em>[<em>n</em>] is maximized when the sequences <em>z</em>[<em>n </em>− <em>i</em>] and <em>p</em>[<em>i</em>]<em>, i </em>= 0<em>,…,N<sub>p </sub></em>− 1<em>, </em>are equal. Let the <em>n </em>which maximizes the correlation be denoted by <em>n</em><sub>max</sub>. Assuming that <em>θ</em>[<em>n</em><sub>max </sub>− <em>i</em>] ≈ <em>θ </em>over the preamble, we have:

<em>N</em><em>p</em>−1

<em>c</em>[<em>n</em>max] = X <em>p</em>∗[<em>i</em>]<em>p</em>[<em>n</em>max − <em>i</em>]<em>e</em><em>jθ</em>[<em>n</em>max−<em>i</em>]                                          (5.13)

<em>i</em>=0

<em>N</em><em>p</em>−1

≈ X <em><span style="text-decoration: line-through">p</span></em><sup>∗</sup>[<em>i</em>]<em>p</em>[<em>n</em><sub>max </sub>− <em>i</em>]<em>e<sup>jθ                                                           </sup></em>(5.14)

<em>i</em>=0

<em>N</em><em>p</em>−1

= <em>e<sup>jθ </sup></em>X <em>p</em><sup>∗</sup>[<em>i</em>]<em>p</em>[<em>n</em><sub>max </sub>− <em>i</em>]<em>.                                          </em>(5.15)

<em>i</em>=0

(5.16)

For the phase of <em>c</em>[<em>n</em><sub>max</sub>] we have:

arg{<em>c</em>[<em>n</em><sub>max</sub>]} = <em>θ.                                                 </em>(5.17)

Thus, the phase of the peak of the correlator can be used as an initial phase estimate.

5.5: YourTasks

<h2>            5.5    Your Tasks</h2>

<ol>

 <li>Create phase noise by implementing the random walk described in (5.3) and apply this phase noise to the signal.</li>

 <li>Modify the phase synchronization unit so that it provides an initial phase estimate according to (5.17).</li>

 <li>Implement the phase synchronization unit and integrate it into your system. Compare your phase estimates with the true values.</li>

 <li>The estimator (5.8) does not take the correlation between the phases into account. But we have seen in Figure 5.2 that the phase varies quite slowly. Suggest a way to improve the estimator by exploiting the correlation.</li>

</ol>