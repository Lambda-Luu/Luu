# 🎓 Tutorial: Real-Time Acoustic Frequency Analyzer

In this tutorial, you will learn how to use **Luu IDE** to build a tool capable of capturing audio from your microphone, processing the signal using digital signal processing (DSP) algorithms, and visualizing the peak frequency (pitch) of your voice in real-time.

---

## 🎯 Objective
Create a script that performs:
1. Audio capture at 44.1 kHz.
2. Waveform visualization (Oscilloscope).
3. Low-frequency noise filtering.
4. Frequency spectrum calculation (FFT).
5. Identification and display of the dominant frequency.

---

## 💻 The Full Code

```kotlin
val f: Double = 44100.0 / 8192  

fun main() {
 
    val reader = AudioRecorder(44100.0, 8192)
    val signal = reader.read().publish()
    
    val signalPlot = findViewByTag<PlotYtsView>("signal")
    val signalCh1 = signalPlot.getChannel("ch1")
    signal.collect { v -> signalCh1.ys(v) }


    val fftPlot = Sys.findViewByTag<PlotYtsView>("fft")    
    val fftCh1 = fftPlot.getChannel("ch1")
    
    val peakText = Sys.findViewByTag<TextView>("peak")

    val fft = signal
        .map { v -> Sys.Filter.butterworthHighPass(v, 80.0, 44100.0) }
        .map { v -> 
            fft(v)
                .subList(0, v.size() / 2)
                .map { c -> c.abs() } 
        }
        .publish()

    fft.collect { v -> fftCh1.ys(v) }
    
    
    fft.map { v -> Sys.DSP.peak(v) * f }
       .collect { v -> peakText.setText(v.format(1) + " Hz") }
}
```

---

## 🚶 Step-by-Step Breakdown

### Step 1: Audio Capture
We use `Sys.AudioRecorder` to open the microphone. When calling `.read()`, Luu generates a constant `Stream` of number lists (`List<Double>`). We use `.publish()` so this same signal can feed both the oscilloscope plot and the FFT process simultaneously.

### Step 2: Visual Interface Interaction
Luu IDE allows you to design interfaces visually. Using `Sys.findViewByTag`, we "connect" our code with UI components (plots, labels, buttons). In this case, we look for a `PlotYtsView` named "signal" to draw the waveform.

### Step 3: Signal Cleaning
Laptop microphone signals often have low-frequency noise (motors, DC offset). We apply a **Butterworth High-Pass** filter at 80 Hz. This ensures that the FFT focuses on the voice rather than background noise.

### Step 4: From Time to Frequency (FFT)
The `Sys.DSP.fft` function converts our list of temporal samples into a list of complex numbers representing the spectrum. Since the spectrum is symmetrical, we only care about the first half (`subList`). To visualize it, we convert the complex numbers to their scalar magnitude using `.map { it.abs() }`.

### Step 5: Finding the Fundamental Frequency
The `Sys.DSP.peak` function returns the position (index) of the highest value in the spectrum. By multiplying this index by our resolution `f` (which depends on the sampling frequency and buffer size), we get the exact value in **Hertz**. Finally, we format the number and show it in a `TextView`.

---

## 🚀 Next Steps
* Try changing the filter value from 80.0 to 200.0 and see how it affects detection.
* Add a `GaugeView` to show the signal volume (RMS).
* Sing a constant note and watch how Luu IDE hits the frequency precisely!
