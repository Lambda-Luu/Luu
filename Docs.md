# 📘 Luu Language Specification

**Version:** 1.1.0  
**Status:** Official Specification  
**Last Updated:** 2026-07-25

---

## 1. Core Philosophy and Syntax

Luu is designed under the premise that **"Safety is Performance"**. In a scientific environment, a runtime crash due to a null pointer can ruin hours of data acquisition. Therefore, Luu enforces strict rules that eliminate entire classes of bugs at compile time.

### 1.1 Variables and Constants: The Immutability First Rule
Luu distinguishes clearly between read-only and mutable references. 

#### `val` (Immutable)
Defines a reference that, once assigned, cannot change. It is the recommended way to define constants, mathematical results, and UI view references.
*   **Safety:** Prevents accidental side effects in long scripts.
*   **Usage:** `val pi = 3.14159`

#### `var` (Mutable)
Defines a reference that can be reassigned. Use it for counters, accumulators, or state variables.
*   **Usage:** `var counter = 0`

### 1.2 The "No Null" Policy
One of Luu's most powerful features is the absolute absence of `null`.
*   **Rule:** A variable cannot exist without a value.
*   **Initialization:** Every declaration must be initialized immediately.
*   **Exception (`lateinit`):** If you cannot initialize a variable at declaration (e.g., global UI references), you can use `lateinit var`. You must ensure it's assigned before any access, or a runtime error will occur.
*   **Result:** You will never encounter a "Null Pointer Exception" in Luu. The interpreter guarantees that if a variable is accessible, it contains data.

### 1.3 Primitive Data Types
Luu uses 64-bit precision for all numeric operations to ensure accuracy in scientific calculations.

*   **`Int`**: 64-bit signed integer. Capable of handling values from -9 quintillion to +9 quintillion. Used for indexing, counts, and discrete logic.
*   **`Double`**: 64-bit IEEE 754 floating point. The standard for all scientific measurements, frequency data, and coordinates.
*   **`Boolean`**: Logical `true` or `false`. Essential for conditional branching and state checking.
*   **`String`**: UTF-8 encoded text. Strings in Luu are immutable and support concatenation via the `+` operator.

### 1.4 Advanced Data Types

#### `Complex`
In instrumentation, phase and magnitude are crucial. Luu treats complex numbers as first-class citizens.
*   **Properties:** `real` (Double), `imag` (Double).
*   **Creation:** `val z = Complex(1.0, -0.5)`
*   **Operations:** Supports standard arithmetic through the `Sys.Math` module.

#### `List<T>`
Dynamic, type-safe arrays. Lists are the primary way to handle signal buffers and data series.
*   **Shorthand Syntax:** `val data = [1.0, 2.0, 3.0]`
*   **Concatenation:** `val combined = listA + listB`
*   **Access:** Zero-indexed via `list[index]`.
*   **Functional API:** Supports `map`, `filter`, and `forEach` for high-level data transformation.
*   **Methods:**
    *   `size()`: Returns the number of elements (Int).
    *   `isEmpty()`: Returns true if the list is empty (Boolean).
    *   `isNotEmpty()`: Returns true if the list is not empty (Boolean).
    *   `get(index)`: Returns the element at the specified index.
    *   `add(value)`: Appends an element to the end of the list.
    *   `add(index, value)`: Inserts an element at the specified index.
    *   `addAll(list)`: Appends all elements from another list.
    *   `addAll(index, list)`: Inserts all elements from another list at the specified index.
    *   `remove(value)`: Removes the first occurrence of the specified element.
    *   `remove(index)`: Removes the element at the specified index.
    *   `clear()`: Removes all elements from the list.
    *   `indexOf(value)`: Returns the index of the first occurrence of the value, or -1 if not found.
    *   `lastIndexOf(value)`: Returns the index of the last occurrence of the value, or -1 if not found.
    *   `contains(value)`: Returns true if the list contains the value.
    *   `containsAll(list)`: Returns true if the list contains all elements of another list.
    *   `subList(fromIndex, toIndex)`: Returns a new list containing elements between the specified indices.
    *   `first()`: Returns the first element.
    *   `first(predicate)`: Returns the first element matching the predicate.
    *   `last()`: Returns the last element.
    *   `last(predicate)`: Returns the last element matching the predicate.
    *   `find(predicate)`: Returns the first element matching the predicate, or `undef` if not found.
    *   `any(predicate)`: Returns true if at least one element matches the predicate.
    *   `all(predicate)`: Returns true if all elements match the predicate.
    *   `none(predicate)`: Returns true if no elements match the predicate.
    *   `count(predicate)`: Returns the number of elements matching the predicate.
    *   `reversed()`: Returns a new list with elements in reversed order.
    *   `shuffled()`: Returns a new list with elements randomly shuffled.
    *   `take(n)`: Returns a new list containing the first n elements.
    *   `drop(n)`: Returns a new list containing all elements except the first n elements.
    *   `sortWith(comparator)`: Sorts the list using the given comparator lambda.
    *   `toStream()`: Converts the list into a data stream.

## 2. Control Flow and Decision Making

Luu provides structured control flow designed to be readable and predictable.

### 2.1 Conditionals: `if` and `else`
Luu follows a standard `if-else` syntax.
*   **Syntax:** `if (condition) { ... } else if (other) { ... } else { ... }`
*   **Note:** Conditions must always evaluate to a `Boolean`. Implicit truthiness (like in C or Python) is not allowed.

### 2.2 Pattern Matching: `when`
The `when` statement is Luu's most powerful branching tool. It can be used in two modes:

#### With Subject (Classic Switch)
Used to compare an object against multiple values.
```kotlin
when (command) {
    "START" -> { startEngine() }
    "STOP"  -> { stopEngine() }
    else    -> { logError() }
}
```

#### Without Subject (Logical Branching)
Acts as a cleaner alternative to multiple `if-else` blocks.
```kotlin
when {
    temperature > 100.0 -> { activateCooling() }
    pressure < 1.0     -> { checkLeaks() }
    else              -> { operationNormal() }
}
```

### 2.3 Iteration and Loops

#### `for` (Range-based)
Used for iterating over discrete steps.
*   **Operator:** `..` creates an inclusive range.
*   **Step:** Optional `step` keyword for custom increments.
*   **Example:** `for (i in 0..100 step 10) { ... }` (0, 10, 20... 100).

#### `while` (Pre-condition)
Repeats as long as the condition is true. Checked at the start.
*   **Example:** `while (systemRunning) { readSensors() }`

#### `do-while` (Post-condition)
Guarantees at least one execution. Condition checked at the end.
*   **Example:** `do { process() } while (shouldContinue)`

## 3. Functions, Lambdas, and Scoping

Luu is a "Lambda-First" language. Functions are treated as variables (First-Class Citizens).

### 3.1 Named Functions
Declared using the `fun` keyword. 
```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```
*   **Return:** If no type is specified, the function returns `Unit`.

### 3.2 Lambdas (Anonymous Functions)
Lambdas are blocks of code that can be stored in variables or passed as parameters.

#### Full Syntax
```kotlin
val multiply = { x: Double, y: Double -> x * y }
```

#### Implicit Parameter Types
If the type is explicitly declared in the variable, types in the body can be omitted:
```kotlin
val multiply: (Double, Double) -> Double = { x, y -> x * y }
```

#### Implicit Return
Lambdas automatically return the value of their last expression. The `return` keyword can be omitted:
```kotlin
val sum: (Double, Double) -> Double = { x, y -> x + y }
```

#### Trailing Lambdas
If the last parameter of a function is a lambda, it can be passed outside the parentheses:
```kotlin
view.setOnClickListener {
    // Code to execute on click
}
```

### 3.3 The "No This" Scoping Rule
Luu eliminates the `this` keyword to simplify access and prevent scoping bugs.
1.  **Local Priority:** The interpreter first looks for variables in the current function/lambda.
2.  **Parent Scopes:** It then looks at parent functions/blocks.
3.  **Class Context:** Finally, it checks properties and methods of the containing class automatically.
*   **Result:** You just write the name of the property. No `this.myProperty` required.

## 4. System Library: (`Sys`)

Common methods for I/O, UI access, and console output.

*   **`Sys.findViewByTag<V>(tag: String): V`**: Retrieves a view instance defined in the editor by its tag.
*   **`Sys.read<T>(path: String): List<T>`**: Reads data from a file and returns it as a List of type T.
*   **`Sys.save(path: String, data: Any)`**: Saves data to a file at the specified path.
*   **`Sys.print(msg: Any)`**: Prints a message to the console without a line break.
*   **`Sys.println(msg: Any)`**: Prints a message to the console followed by a line break.

---

## 5. UI Components Reference

UI components are retrieved via `Sys.findViewByTag<T>(tag)`. Once retrieved, they act as objects with the following methods:

### 5.1 ButtonView
*   **`setOnClickListener(listener: () -> Void)`**: Sets a callback for click events.

### 5.2 KnobView & SliderView
*   **`setValue(value: Double)`**: Sets the current numeric value.
*   **`setOnValueChangeListener(listener: (Double) -> Void)`**: Sets a callback for value changes.
*   **`toStream(): Stream<Double>`**: Returns a stream that emits values on every change.

### 5.3 XYPadView
*   **`setValues(x: Double, y: Double)`**: Sets the crosshair position (0.0 to 1.0).
*   **`setOnValueChangeListener(listener: (Double, Double) -> Void)`**: Callback with new X and Y coordinates.

### 5.4 DropdownMenuView & RadioButtonView
*   **`setSelectedIndex(index: Int)`**: Selects an option by its index.
*   **`setOnSelectionChangeListener(listener: (Int) -> Void)`**: Callback with the new selected index.

### 5.5 LedView
*   **`setOn(isOn: Boolean)`**: Controls the LED state.
*   **`setColor(color: Int)`**: Changes the LED color.

### 5.6 LedMatrixView
*   **`setOn(row: Int, col: Int, isOn: Boolean)`**: Controls a specific LED.
*   **`setMatrix(data: List<List<Boolean>>)`**: Updates the entire grid.
*   **`setLedColor(color: Int)`**: Sets the color for the active LEDs.
*   **`clear()`**: Turns off all LEDs.

### 5.7 LevelMeterView & GaugeView
*   **`setLevel(level: Double)`** / **`setValue(value: Double)`**: Updates the visual indicator.

### 5.8 CanvasView
*   **`addCircle(x: Double, y: Double, radius: Double, color: Int)`**: Draws a circle.
*   **`addLine(x1: Double, y1: Double, x2: Double, y2: Double, color: Int, thickness: Int)`**: Draws a line.
*   **`clear()`**: Clears the canvas.

---

## 6. System Library: Math (`Sys.Math`)

The `Math` module provides high-performance arithmetic and trigonometric functions.

### 6.1 Constants
*   **`PI`**: The ratio of a circle's circumference to its diameter (~3.14159).
*   **`E`**: Euler's number (~2.71828).

### 6.2 Power and Logarithms
*   **`abs(v: Double): Double`**: Returns the absolute value.
*   **`sqrt(v: Double): Double`**: Square root.
*   **`pow(v: Double, p: Double): Double`**: Raises `v` to the power of `p`.
*   **`exp(v: Double): Double`**: Returns `e` raised to the power of `v`.
*   **`log10(v: Double): Double`**: Base-10 logarithm.
*   **`ln(v: Double): Double`**: Natural logarithm (base `e`).

### 6.3 Trigonometry (Radians)
All trigonometric functions expect angles in **radians**.
*   **`sin(v)`, `cos(v)`, `tan(v)`**: Standard functions.
*   **`asin(v)`, `acos(v)`, `atan(v)`**: Inverse functions.
*   **`atan2(y: Double, x: Double): Double`**: Returns the angle theta from the conversion of rectangular coordinates (x, y) to polar coordinates.

### 6.4 Rounding and Extremes
*   **`ceil(v)`, `floor(v)`, `round(v)`**: Standard rounding to the nearest integer.
*   **`min(a, b)`, `max(a, b)`**: Returns the lesser or greater of two values.

### 6.5 Advanced Math & Conversions
*   **`factorial(n: Int): Double`**: Computes `n!`.
*   **`combinations(n: Int, k: Int): Double`**: Computes "n choose k".
*   **`toDb(amplitude: Double): Double`**: Converts linear amplitude to Decibels ($20 \cdot \log10(A)$).
*   **`fromDb(db: Double): Double`**: Converts Decibels back to linear amplitude.

---

## 7. System Library: DSP (`Sys.DSP`)

The Digital Signal Processing module is the core of Luu's virtual instrumentation capabilities.

### 7.1 Spectral and Frequency Analysis
*   **`fft(data: List<Double>): List<Complex>`**: Computes the Fast Fourier Transform.
*   **`stft(data, windowSize, hopSize): List<List<Complex>>`**: Short-Time Fourier Transform. Returns a spectrogram.
*   **`cepstrum(data): List<Double>`**: Computes the real cepstrum of a signal.
*   **`hilbert(data): List<Complex>`**: Computes the analytic signal using the Hilbert transform.

### 7.2 Signal Characterization
*   **`peak(data): Int`**: Returns the index of the highest peak.
*   **`findPeaks(data, threshold): List<Int>`**: Returns indices of all peaks above the threshold.
*   **`envelope(data, windowSize): List<Double>`**: Computes the signal envelope.
*   **`autocorrelation(data): List<Double>`**: Computes the autocorrelation sequence.
*   **`zcr(data): Double`**: Zero-Crossing Rate.
*   **`zeroCrossings(data): List<Int>`**: Returns the indices where zero-crossings occur.
*   **`thd(data): Double`**: Total Harmonic Distortion.
*   **`snr(data): Double`**: Signal-to-Noise Ratio in dB.
*   **`crestFactor(data): Double`**: Ratio of peak values to RMS.
*   **`riseTime(data, sampleRate): Double`**: Time taken for a signal to rise from 10% to 90% of its steady-state value.

### 7.3 Signal Generation
*   **`sinWave(n, freq, fs, phase, amp)`**: Generates `n` samples of a sine wave.
*   **`squareWave(n, freq, fs, phase, amp)`**: Generates a square wave.
*   **`triangleWave(n, freq, fs, phase, amp)`**: Generates a triangle wave.
*   **`sawtoothWave(n, freq, fs, phase, amp)`**: Generates a sawtooth wave.
*   **`whiteNoise(n, amp)`**: Generates Gaussian white noise.
*   **`pinkNoise(n, amp)`**: Generates pink noise (1/f noise).
*   **`chirp(n, f0, f1, fs, duration, amp)`**: Generates a frequency-swept signal.

### 7.4 Pitch Detection and Estimation
*   **`yin(data, fs, threshold): Double`**: Pitch detection using the YIN algorithm.
*   **`mpm(data, fs, threshold): Double`**: Pitch detection using the McLeod Pitch Method.
*   **`amdf(data, fs, minFreq, maxFreq): Double`**: Average Magnitude Difference Function.
*   **`hps(magnitudeSpectrum, downsampleFactors): Int`**: Harmonic Product Spectrum for fundamental frequency estimation.

### 7.5 Windows and Transforms
*   **`hann(n): List<Double>`**: Generates a Hann window of length `n`.
*   **`blackman(n): List<Double>`**: Generates a Blackman window of length `n`.
*   **`dwt(data, levels): List<Double>`**: Discrete Wavelet Transform.
*   **`convolve(signal, kernel): List<Double>`**: Performs linear convolution of two signals.
*   **`resample(data, newSize): List<Double>`**: Changes the sampling rate of the signal.

### 7.6 Parametric Estimation (Subspace methods)
*   **`music(data, m, numSignals): List<Double>`**: Multiple Signal Classification algorithm for frequency estimation.
*   **`esprit(data, m, numSignals): List<Double>`**: Estimation of Signal Parameters via Rotational Invariance Techniques.

---

## 8. System Library: Filters (`Sys.Filter`)

The `Filter` module provides digital signal filtering algorithms to reduce noise and isolate components.

### 8.1 Smoothing and Averages
*   **`ema(data, alpha): List<Double>`**: Exponential Moving Average. `alpha` (0.0 to 1.0) controls the smoothing factor.
*   **`sma(data, window): List<Double>`**: Simple Moving Average with a sliding window of the specified size.
*   **`median(data, window): List<Double>`**: Median filter, excellent for removing "spike" noise (outliers).

### 8.2 Frequency Filters
*   **`lowPass(data, cutoff, fs): List<Double>`**: First-order low-pass filter.
*   **`highPass(data, cutoff, fs): List<Double>`**: First-order high-pass filter.
*   **`butterworthLowPass(data, cutoff, fs): List<Double>`**: Second-order Butterworth low-pass filter for a flatter response.
*   **`butterworthHighPass(data, cutoff, fs): List<Double>`**: Second-order Butterworth high-pass filter.
*   **`notch(data, freq, fs, q): List<Double>`**: Rejects a narrow band of frequencies around `freq` (e.g., 50Hz hum).

### 8.3 Advanced Filters
*   **`kalman(data, q, r, initEst, initErr): List<Double>`**: Simple 1D Kalman filter for optimal estimation under noise.

---

## 9. System Library: Statistics (`Sys.Stats`)

The `Stats` module provides a comprehensive suite of tools for data characterization and hypothesis testing, essential for analyzing experimental results.

### 9.1 Descriptive Statistics
*   **`mean(data: List<Double>): Double`**: Arithmetic average.
*   **`median(data): Double`**: The middle value of the sorted data set.
*   **`stdDev(data): Double`**: Standard deviation (measure of dispersion).
*   **`variance(data): Double`**: Variance of the sample.
*   **`kurtosis(data)` & `skewness(data)`**: Measures of the "shape" of the distribution.
*   **`linearRegression(x, y)`**: Returns `[slope, intercept]` for the best-fit line.

### 9.2 Inferential & Testing
*   **`welchTTest(a, b): Double`**: Performs a T-Test for two samples with potentially unequal variances. Returns the p-value.
*   **`percentile(data, p): Double`**: Returns the value below which `p` percent of the data falls.
*   **`confidenceInterval(data, level)`**: Returns the [lower, upper] bounds for a given confidence level (e.g., 0.95).

---

## 10. System Library: Numeric (`Sys.Numeric`)

Advanced calculus and mathematical modeling using high-precision numerical methods.

### 10.1 Calculus & ODEs
*   **`integrate(data, fs): List<Double>`**: Cumulative trapezoidal integration.
*   **`differentiate(data, fs): List<Double>`**: Central difference differentiation.
*   **`rungeKutta4(y0, t0, tEnd, h, f)`**: Solves a first-order Ordinary Differential Equation using the RK4 method.

### 10.2 Modeling & Fitting
*   **`linearRegression(x, y)`**: Returns `[slope, intercept]` for the best-fit line.
*   **`polyFit(x, y, degree)`**: Fits a polynomial of the specified degree. Returns coefficients from highest power to constant.
*   **`spline(x, y, targetX)`**: Natural Cubic Spline interpolation. Smooths data through points.
*   **`parabolicPeak(y0, y1, y2)`**: Estimates the sub-sample peak position and amplitude using three points.

### 10.3 Optimization
*   **`findRoot(a, b, tol, f)`**: Finds the value $x$ where $f(x) = 0$ using the bisection method.
*   **`minimize(a, b, tol, f)`**: Finds the local minimum of a function within a range.

---

## 11. System Library: Linear Algebra (`Sys.Matrix`)

The `Matrix` class provides support for matrix operations, essential for advanced data processing and physics simulations.

### 11.1 Creation
*   **`Matrix(rows: Int, cols: Int, data: List<Double>)`**: Creates a new matrix. The data list must have exactly `rows * cols` elements in row-major order.

### 11.2 Operations
*   **`add(other: Matrix): Matrix`**: Returns the sum of two matrices.
*   **`multiply(other: Matrix): Matrix`**: Returns the matrix product.
*   **`det(): Double`**: Calculates the determinant of a square matrix.
*   **`inv(): Matrix`**: Calculates the inverse of a square matrix.
*   **`eigenvalues(): List<Double>`**: Estimates real eigenvalues using the QR algorithm.

### 11.3 Properties
*   **`rows(): Int`**: Returns the number of rows.
*   **`cols(): Int`**: Returns the number of columns.

---

## 12. System Library: Geometry (`Sys.Geo`)

Computational geometry for spatial analysis and 3D projection.

*   **Types**:
    *   `Point(x, y, z)`: Spatial coordinates. `p.distanceTo(other)`.
    *   `Vector(x, y, z)`: Direction and magnitude. `v.length()`, `v.normalize()`, `v.dot(other)`, `v.cross(other)`.
    *   `Line(origin, dir)`: `line.pointAt(t)`, `line.distanceTo(p)`, `line.intersect2D(other)`.
    *   `Plane(p, normal)`: `plane.distanceTo(p)`, `plane.intersect(line)`, `plane.contains(p)`.
    *   `Circle(center, r)`: `c.contains(p)`, `c.intersects(other)`, `c.intersectionArea(other)`.
    *   `Sphere(center, r)`: `s.contains(p)`, `s.intersects(other)`.
    *   `Matrix4`: 4x4 matrix for affine transforms.
        *   `identity()`, `translate(x, y, z)`, `scale(x, y, z)`, `rotateX/Y/Z(rad)`, `skew(sx, sy)`.
        *   `multiply(other)`, `mapPoint(p)`.
        *   `perspective(fov, aspect, near, far)`: 3D to 2D projection matrix.

## 13. System Library: Hardware (`Sys.DAQmx`)

Direct control of National Instruments devices (Desktop only).

*   `Sys.DAQmx()`: Creates a new DAQ controller instance.
*   `daq.writeDigitalLine(deviceLine, state)`: Writes `true/false` to a specific channel (e.g., `"Dev1/port0/line0"`).

---

## 14. Real-Time Data Streams (`Sys.Stream`)

The `Stream` module is the core engine for reactive programming in Luu. It handles the continuous flow of data from generators or hardware to the UI.

### 14.1 Source Generators
*   **`sine(freq, amp)`**: Continuous sine wave generator.
*   **`audioIn()`**: Streams data from the system's default microphone.
*   **`periodic(millis)`**: Emits a tick event every X milliseconds.

### 14.2 Operators (Pipelines)
*   **`.delay(ms)`**: Throttles the data flow to the specified rate.
*   **`.map { v -> ... }`**: Transforms each data point in the stream.
*   **`.window(size)`**: Groups individual points into lists (buffers) for batch processing (e.g., for FFT).

### 14.3 Sinks (Consumption)
*   **`.collect { v -> ... }`**: The final step in a pipeline. Executes logic for every value received. This is where you usually update UI components.

---

## 15. IDE Productivity and Shortcuts

The Luu IDE includes several features to speed up your instrumentation workflow.

### 15.1 Smart Autocomplete
*   **Tab Key**: When a red underline (error) appears, press `Tab` to automatically insert the first suggested correction. If the suggestion is a function, the cursor will be placed inside the parentheses.
*   **4-Space Indent**: Pressing `Tab` without active suggestions will insert 4 spaces for consistent code formatting.

### 15.2 Keyboard Shortcuts
*   **`Ctrl + N`** (or `Cmd + N`): Create a new file in the current directory.
*   **`Ctrl + P`** (or `Cmd + P`): Launch the Project Wizard to create a new instrumentation project.
*   **`Ctrl + S`**: Manual save (though the IDE performs auto-save every 5 seconds).
*   **`Ctrl + Click`** (on a symbol): Jump to the definition of a variable or function.

### 15.3 File Types
*   **`.lu`**: Luu logic scripts. Contains your `fun main()` and data processing pipelines.
*   **`.vw`**: Visual View files. These are managed by the Visual Editor and should not be edited manually.
