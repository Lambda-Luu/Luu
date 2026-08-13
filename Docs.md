# 📘 Luu Language Specification

**Version:** 1.1.5  
**Status:** Official Specification  
**Last Updated:** 2026-08-13

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
    *   `distinct()`: Returns a new list containing only unique elements.
    *   `distinctBy(selector)`: Returns a new list containing unique elements based on the result of the selector lambda.
    *   `sortWith(comparator)`: Sorts the list using the given comparator lambda.
    *   `toStream()`: Converts the list into a data stream.

#### `Map<K, V>`
Key-value collections for efficient data retrieval.
*   **Creation:** `val registry = Map<String, Int>()`
*   **Methods:**
    *   `size()`: Returns the number of entries (Int).
    *   `isEmpty()`: Returns true if the map is empty (Boolean).
    *   `isNotEmpty()`: Returns true if the map is not empty (Boolean).
    *   `get(key)`: Returns the value associated with the key, or `undef` if not found.
    *   `add(key, value)`: Adds or updates a key-value pair.
    *   `remove(key)`: Removes the entry for the specified key.
    *   `clear()`: Removes all entries from the map.
    *   `contains(key)`: Returns true if the map contains the specified key.
    *   `keys()`: Returns a list containing all keys in the map.
    *   `values()`: Returns a list containing all values in the map.
    *   **Functional API:**
        *   `forEach(action)`: Executes a lambda for each entry `(key, value)`.
        *   `map(func)`: Transforms each entry into a list element.
        *   `filter(predicate)`: Returns a new map with entries matching the condition.
        *   `find(predicate)`: Returns the first `Entry` matching the condition.
        *   `any(predicate)` / `all(predicate)` / `none(predicate)`: Logical checks across all entries.
        *   `count(predicate)`: Counts entries matching the condition.

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

---

## 4. System Library: Basic (`Sys`)

Common methods for I/O, UI access, and console output.

*   **`findViewByTag<V>(tag: String): V`**: Retrieves a view instance defined in the editor by its tag.
*   **`read<T>(path: String): List<T>`**: Reads data from a file and returns it as a List of type T. Supports JSON, CSV, and .doubles formats.
*   **`save(path: String, data: Any)`**: Saves data to a file at the specified path.
*   **`print(msg: Any)`**: Prints a message to the console without a line break.
*   **`println(msg: Any)`**: Prints a message to the console followed by a line break.

---

## 5. System Library: Math (`Sys.Math`)

The `Math` module provides high-performance arithmetic and trigonometric functions.

### 5.1 Constants
*   **`PI`**: The ratio of a circle's circumference to its diameter (~3.14159).
*   **`E`**: Euler's number (~2.71828).
*   **`C`**: Speed of light in m/s (299,792,458).
*   **`G`**: Gravitational acceleration in m/s² (9.80665).

### 5.2 Power and Logarithms
*   **`abs(v: Double): Double`**: Returns the absolute value.
*   **`sqrt(v: Double): Double`**: Square root.
*   **`pow(v: Double, p: Double): Double`**: Raises `v` to the power of `p`.
*   **`exp(v: Double): Double`**: Returns `e` raised to the power of `v`.
*   **`log10(v: Double): Double`**: Base-10 logarithm.
*   **`ln(v: Double): Double`**: Natural logarithm (base `e`).

### 5.3 Trigonometry (Radians)
All trigonometric functions expect angles in **radians**.
*   **`sin(v)`, `cos(v)`, `tan(v)`**: Standard functions.
*   **`asin(v)`, `acos(v)`, `atan(v)`**: Inverse functions.
*   **`atan2(y: Double, x: Double): Double`**: Returns the angle theta from the conversion of rectangular coordinates (x, y) to polar coordinates.

### 5.4 Rounding and Extremes
*   **`ceil(v)`, `floor(v)`, `round(v)`**: Standard rounding to the nearest integer.
*   **`min(a, b)`, `max(a, b)`**: Returns the lesser or greater of two values.
*   **`min(list)`, `max(list)`**: Returns the minimum or maximum value in a list.

### 5.5 Advanced Math & Conversions
*   **`factorial(n: Int): Double`**: Computes `n!`.
*   **`combinations(n: Int, k: Int): Double`**: Computes "n choose k".
*   **`lerp(a, b, t)`**: Linear interpolation between `a` and `b` by factor `t`.
*   **`map(v, inMin, inMax, outMin, outMax)`**: Maps a value from one range to another.
*   **`toDb(amplitude: Double): Double`**: Converts linear amplitude to Decibels ($20 \cdot \log10(A)$).
*   **`fromDb(db: Double): Double`**: Converts Decibels back to linear amplitude.
*   **`rms(list): Double`**: Root Mean Square energy of a signal.
*   **`normalize(list): List<Double>`**: Normalizes a list so the maximum absolute value is 1.0.
*   **`magnitude(list): Double`**: Euclidean magnitude (norm) of a vector/list.

### 5.6 Geometry & Coordinates
*   **`polarToCartesian(r, theta): List<Double>`**: Converts polar to [x, y].
*   **`cartesianToPolar(x, y): List<Double>`**: Converts [x, y] to [r, theta].
*   **`dot(listA, listB): Double`**: Dot product of two vectors.
*   **`cross3D(listA, listB): List<Double>`**: Cross product of two 3D vectors.

---

## 6. System Library: DSP (`Sys.DSP`)

Digital Signal Processing module for virtual instrumentation.

### 6.1 Spectral Analysis
*   **`fft(data: List<Double>): List<Complex>`**: Fast Fourier Transform.
*   **`stft(data, windowSize, hopSize): List<List<Complex>>`**: Short-Time Fourier Transform. Returns a spectrogram.
*   **`cepstrum(data): List<Double>`**: Real cepstrum.
*   **`hilbert(data): List<Complex>`**: Analytic signal using Hilbert transform.
*   **`spectralFlatness(data): Double`**: Measures the "flatness" of the spectrum (0.0=tonal, 1.0=white noise).
*   **`flatnessCoefficient(magnitudes, start?, end?): Double`**: Low-level SFM calculation over a range of FFT bins.

### 6.2 Signal Characterization
*   **`peak(data): Int`**: Index of the highest peak.
*   **`peakValue(data): Double`**: Magnitude of the highest peak.
*   **`findPeaks(data, threshold): List<Int>`**: Indices of all peaks above threshold.
*   **`envelope(data, windowSize): List<Double>`**: RMS envelope.
*   **`autocorrelation(data): List<Double>`**: Autocorrelation sequence.
*   **`zcr(data): Double`**: Zero-Crossing Rate.
*   **`zeroCrossings(data): List<Int>`**: Indices of zero-crossings.
*   **`thd(data): Double`**: Total Harmonic Distortion.
*   **`snr(data): Double`**: Signal-to-Noise Ratio in dB.
*   **`crestFactor(data): Double`**: Peak-to-RMS ratio.
*   **`riseTime(data, sampleRate): Double`**: Time from 10% to 90% of steady-state.

### 6.3 Signal Generation
*   **`sinWave(n, freq, fs, phase, amp)`**: Sine wave generation.
*   **`squareWave(n, freq, fs, phase, amp)`**: Square wave generation.
*   **`triangleWave(n, freq, fs, phase, amp)`**: Triangle wave generation.
*   **`sawtoothWave(n, freq, fs, phase, amp)`**: Sawtooth wave generation.
*   **`whiteNoise(n, amp)`**: Gaussian white noise.
*   **`pinkNoise(n, amp)`**: 1/f noise.
*   **`chirp(n, f0, f1, fs, duration, amp)`**: Frequency-swept signal.

### 6.4 Pitch Detection
*   **`yin(data, fs, threshold)`**: YIN algorithm.
*   **`mpm(data, fs, threshold)`**: McLeod Pitch Method.
*   **`amdf(data, fs, minFreq, maxFreq)`**: Average Magnitude Difference Function.
*   **`hps(magnitudeSpectrum, downsampleFactors)`**: Harmonic Product Spectrum.

---

## 7. System Library: Filters (`Sys.Filter`)

Digital filtering algorithms.

*   **`ema(data, alpha)`**: Exponential Moving Average.
*   **`sma(data, window)`**: Simple Moving Average.
*   **`median(data, window)`**: Median filter (spike removal).
*   **`lowPass(data, cutoff, fs)`**: 1st order Low-pass.
*   **`highPass(data, cutoff, fs)`**: 1st order High-pass.
*   **`butterworthLowPass(data, cutoff, fs)`**: 2nd order Butterworth.
*   **`butterworthHighPass(data, cutoff, fs)`**: 2nd order Butterworth.
*   **`notch(data, freq, fs, q)`**: Notch filter.
*   **`kalman(data, q, r, initEst, initErr)`**: 1D Kalman filter.

---

## 8. System Library: Statistics (`Sys.Stats`)

*   **`mean(data)`**, **`variance(data)`**, **`stdDev(data)`**, **`rms(data)`**: Descriptive stats.
*   **`histogram(data, bins)`**: Returns frequency counts per bin.
*   **`entropy(data)`**: Shannon entropy.
*   **`percentile(data, p)`**: Value at percentile `p`.
*   **`welchTTest(a, b)`**: P-value for Welch's T-test.
*   **`correlationCoefficient(a, b)`**: Pearson's R.
*   **`linearRegression(x, y)`**: Returns [slope, intercept].
*   **`confidenceInterval(data)`**: Returns [lower, upper] at 95%.
*   **`skewness(data)`**, **`kurtosis(data)`**: Shape metrics.

---

## 9. System Library: Numeric (`Sys.Numeric`)

*   **`integrate(data, fs)`**: Trapezoidal integration.
*   **`differentiate(data, fs)`**: Central difference.
*   **`detrend(data)`**: Removes linear trend.
*   **`polyFit(x, y, degree)`**: Polynomial least-squares fitting.
*   **`spline(x, y, targetX)`**: Natural Cubic Spline interpolation.
*   **`rungeKutta4(y0, t0, tEnd, h, f)`**: ODE solver.
*   **`findRoot(a, b, tol, f)`**: Bisection root finding.
*   **`minimize(a, b, tol, f)`**: Golden-section minimization.
*   **`parabolicPeak(y0, y1, y2)`**: Sub-sample peak estimation.

---

## 10. System Library: Geometry (`Sys.Geo`)

Geometry types accessible as `Sys.<Type>`.

*   **`Point(x, y, z?)`**: `p.distanceTo(other)`.
*   **`Vector(x, y, z?)`**: `v.length()`, `v.normalize()`, `v.dot(other)`, `v.cross(other)`.
*   **`Line(origin: Point, dir: Vector)`**: `line.pointAt(t)`, `line.distanceTo(p)`, `line.intersect2D(other)`.
*   **`Plane(p: Point, normal: Vector)`**: `plane.distanceTo(p)`, `plane.intersect(line)`.
*   **`Circle(center: Point, r: Double)`**: `c.contains(p)`, `c.intersects(other)`.
*   **`Sphere(center: Point, r: Double)`**: `s.contains(p)`.
*   **`Matrix4()`**: 4x4 matrix for transforms. Supports `identity()`, `translate()`, `rotateX/Y/Z()`, `scale()`, `multiply()`, `mapPoint()`.

---

## 11. System Library: Hardware (`Sys.DAQmx`)

*   **`DAQmx()`**: Creates a controller instance.
    *   `daq.writeDigitalLine(deviceLine, state)`: Writes to a NI-DAQmx digital channel.

---

## 12. UI Components Reference

UI components are retrieved via `Sys.findViewByTag<T>(tag)`.

### 12. UI Components Reference

UI components are retrieved via `Sys.findViewByTag<T>(tag)`. You can find the exact class name (`T`) of a component by selecting it in the visual editor and looking at the **"type"** property in the attributes panel.

### 12.1 ButtonView
*   **`setOnClickListener(listener: () -> Void)`**: Sets a callback for click events.

### 12.2 KnobView & SliderView
*   **`setValue(value: Double)`**: Sets the current numeric value.
*   **`setOnValueChangeListener(listener: (Double) -> Void)`**: Sets a callback for value changes.
*   **`toStream(): Stream<Double>`**: Returns a stream that emits values on every change.

### 12.3 XYPadView
*   **`setValues(x: Double, y: Double)`**: Sets the crosshair position (0.0 to 1.0).
*   **`setOnValueChangeListener(listener: (Double, Double) -> Void)`**: Callback with new X and Y coordinates.

### 12.4 DropdownMenuView & RadioButtonView
*   **`setSelectedIndex(index: Int)`**: Selects an option by its index.
*   **`setOnSelectionChangeListener(listener: (Int) -> Void)`**: Callback with the new selected index.

### 12.5 LedView
*   **`setOn(isOn: Boolean)`**: Controls the LED state.
*   **`setColor(color: Int)`**: Changes the LED color.

### 12.6 LedMatrixView
*   **`setOn(row: Int, col: Int, isOn: Boolean)`**: Controls a specific LED.
*   **`setMatrix(data: List<List<Boolean>>)`**: Updates the entire grid.
*   **`setLedColor(color: Int)`**: Sets the color for the active LEDs.
*   **`clear()`**: Turns off all LEDs.

### 12.7 Plot Y-ts & Plot XY
*   **`getChannel(name: String): Channel`**: Retrieves a channel instance for data feeding.
    *   `channel.setValue(v: Double)`: Adds a single point.
    *   `channel.setValue(list: List<Double>)`: Adds multiple points.
    *   `channel.setStrokeSize(size: Int)`: Updates line thickness.

---

## 13. Real-Time Data Streams (`Sys.Stream`)

### 13.1 Source Generators
*   **`sine(freq, amp)`**: Continuous sine wave generator.
*   **`audioIn(sampleRate?, bufferSize?)`**: Streams data from the microphone.
*   **`periodic(millis)`**: Emits a tick event every X milliseconds.

### 13.2 Operators (Pipelines)
*   **`.delay(ms)`**: Throttles the data flow.
*   **`.map { v -> ... }`**: Transforms each data point.
*   **`.onEach { v -> ... }`**: Executes logic without transforming data.
*   **`.filter { v -> ... }`**: Emits only elements that satisfy the given condition.
*   **`.skip(n)`**: Skips the first `n` elements.
*   **`.take(n)`**: Emits only the first `n` elements and stops.
*   **`.step(n)`**: Emits 1 out of every `n` elements.
*   **`.buffer(count, skip?)`**: Groups points into lists.
*   **`.zip(other)`**: Combines elements from two streams into Pairs.
*   **`.transform { source, emitter -> ... }`**: Custom transformation logic.

### 13.3 Sinks (Consumption)
*   **`.collect { v -> ... }`**: Final step to consume values and update UI.

---

## 14. File Formats Reference

Luu supports several text-based formats for data persistence.

*   **CSV (.csv)**: Comma-separated values.
*   **JSON (.json)**: Standard object notation.
*   **DOUBLES (.doubles)**: Comma-separated decimal numbers.
*   **DOUBLES_TIME (.dt)**: `time:value` pairs.
