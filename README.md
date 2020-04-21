# 1. Trigger Word Detection

Trigger word detection is the technology that allows devices like Amazon Alexa, Google Home, Apple Siri, and Baidu DuerOS to wake up upon hearing a certain word.
For this exercise, our trigger word will be "Activate." Every time it hears you say "activate," it will make a "chiming" sound.
By the end of this assignment, you will be able to record a clip of yourself talking, and have the algorithm trigger a chime when it detects you saying "activate."
After completing this assignment, perhaps you can also extend it to run on your laptop so that every time you say "activate" it starts up your favorite app, or turns on a network connected lamp in your house, or triggers some other event?

## From audio recordings to spectrograms
What really is an audio recording?

A microphone records little variations in air pressure over time, and it is these little variations in air pressure that your ear also perceives as sound.
You can think of an audio recording is a long list of numbers measuring the little air pressure changes detected by the microphone.
We will use audio sampled at 44100 Hz (or 44100 Hertz).
This means the microphone gives us 44,100 numbers per second.
Thus, a 10 second audio clip is represented by 441,000 numbers (= $10 \times 44,100$).
Spectrogram
It is quite difficult to figure out from this "raw" representation of audio whether the word "activate" was said.
In order to help your sequence model more easily learn to detect trigger words, we will compute a spectrogram of the audio.
The spectrogram tells us how much different frequencies are present in an audio clip at any moment in time.
If you've ever taken an advanced class on signal processing or on Fourier transforms:
A spectrogram is computed by sliding a window over the raw audio signal, and calculating the most active frequencies in each window using a Fourier transform.
If you don't understand the previous sentence, don't worry about it.


# 2. Neural Machine Translation

## Translating human readable dates into machine readable dates
The model you will build here could be used to translate from one language to another, such as translating from English to Hindi. However, language translation requires massive datasets and usually takes days of training on GPUs. To give you a place to experiment with these models even without using massive datasets, we will instead use a simpler "date translation" task.

The network will input a date written in a variety of possible formats (e.g. "the 29th of August 1958", "03/30/1968", "24 JUNE 1987") and translate them into standardized, machine readable dates (e.g. "1958-08-29", "1968-03-30", "1987-06-24"). We will have the network learn to output dates in the common machine-readable format YYYY-MM-DD.


Here are some properties of the model that you may notice:

There are two separate LSTMs in this model (see diagram on the left). Because the one at the bottom of the picture is a Bi-directional LSTM and comes before the attention mechanism, we will call it pre-attention Bi-LSTM. The LSTM at the top of the diagram comes after the attention mechanism, so we will call it the post-attention LSTM. The pre-attention Bi-LSTM goes through  Tx  time steps; the post-attention LSTM goes through  Ty  time steps.

The post-attention LSTM passes  s⟨t⟩,c⟨t⟩  from one time step to the next. In the lecture videos, we were using only a basic RNN for the post-activation sequence model, so the state captured by the RNN output activations  s⟨t⟩ . But since we are using an LSTM here, the LSTM has both the output activation  s⟨t⟩  and the hidden cell state  c⟨t⟩ . However, unlike previous text generation examples (such as Dinosaurus in week 1), in this model the post-activation LSTM at time  t  does will not take the specific generated  y⟨t−1⟩  as input; it only takes  s⟨t⟩  and  c⟨t⟩  as input. We have designed the model this way, because (unlike language generation where adjacent characters are highly correlated) there isn't as strong a dependency between the previous character and the next character in a YYYY-MM-DD date.

We use  a⟨t⟩=[a→⟨t⟩;a←⟨t⟩]  to represent the concatenation of the activations of both the forward-direction and backward-directions of the pre-attention Bi-LSTM.

The diagram on the right uses a RepeatVector node to copy  s⟨t−1⟩ 's value  Tx  times, and then Concatenation to concatenate  s⟨t−1⟩  and  a⟨t⟩  to compute  e⟨t,t′ , which is then passed through a softmax to compute  α⟨t,t′⟩ . We'll explain how to use RepeatVector and Concatenation in Keras below.

