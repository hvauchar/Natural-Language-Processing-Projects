# Trigger Word Detection

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

