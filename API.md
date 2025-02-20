# API Integration and Configuration

This document provides information on how to integrate and configure the Acoular project's API.

## API Integration

To integrate the Acoular API into your project, follow these steps:

1. **Install Acoular**: Ensure that Acoular is installed in your environment. You can install it using `conda` or `pip` as described in the [README.md](README.md) file.

2. **Import Acoular**: Import the necessary modules from Acoular in your Python script.

    ```python
    import acoular as ac
    ```

3. **Set Up Microphone Geometry**: Define the microphone geometry by specifying the microphone positions.

    ```python
    from os.path import join, split

    micgeofile = join(split(ac.__file__)[0], 'xml', 'array_64.xml')
    mg = ac.MicGeom(file=micgeofile)
    ```

4. **Load or Generate Data**: Load the recorded data or generate test data using Acoular's demo functions.

    ```python
    ts = ac.TimeSamples(name='your_data_file.h5')
    ```

5. **Configure Signal Processing**: Set up the signal processing chain by configuring the necessary components.

    ```python
    ps = ac.PowerSpectra(source=ts, block_size=128, window='Hanning')
    rg = ac.RectGrid(x_min=-0.2, x_max=0.2, y_min=-0.2, y_max=0.2, z=0.3, increment=0.01)
    st = ac.SteeringVector(grid=rg, mics=mg)
    bb = ac.BeamformerBase(freq_data=ps, steer=st)
    ```

6. **Perform Beamforming**: Perform beamforming to generate acoustic maps.

    ```python
    pm = bb.synthetic(8000, 3)
    ```

7. **Visualize Results**: Visualize the results using a plotting library such as Matplotlib.

    ```python
    import matplotlib.pylab as plt

    Lm = ac.L_p(pm)
    plt.imshow(Lm.T, origin='lower', vmin=Lm.max()-10, extent=rg.extend(), interpolation='bicubic')
    plt.title('Beamformer (base) for 8000 Hz')
    plt.xlabel('x in m')
    plt.ylabel('y in m')
    plt.colorbar(label=r'$L_p$')
    plt.show()
    ```

## API Configuration

The Acoular API provides several configuration options to customize the behavior of the library. These options can be set using the `acoular.config` object.

### Global Caching

The `global_caching` attribute controls the caching behavior of Acoular classes. It can be set to one of the following values:

- `'individual'`: Acoular classes handle caching behavior individually.
- `'all'`: Acoular classes cache everything and read from cache if possible.
- `'none'`: Acoular classes do not cache results. Cache files are not created.
- `'readonly'`: Acoular classes do not actively cache, but read from cache if existing.
- `'overwrite'`: Acoular classes replace existing cache file content with new data.

Example:

```python
acoular.config.global_caching = 'overwrite'
```

### HDF5 Library

The `h5library` attribute specifies the package used to read and write HDF5 files. It can be set to `'pytables'`, `'tables'`, or `'h5py'`.

Example:

```python
acoular.config.h5library = 'h5py'
```

### Cache Directory

The `cache_dir` attribute defines the path to the directory containing Acoular's cache files. If the specified directory does not exist, it will be created.

Example:

```python
acoular.config.cache_dir = '/path/to/cache_dir'
```

### Working Directory

The `td_dir` attribute defines the working directory containing data files. It is used only by the `acoular.tprocess.WriteH5` class.

Example:

```python
acoular.config.td_dir = '/path/to/working_dir'
```

### TraitsUI

The `use_traitsui` attribute determines whether the user has access to TraitsUI features. It defaults to `False`.

Example:

```python
acoular.config.use_traitsui = True
```

### Checking for Installed Packages

The `config` object provides several attributes to check if certain packages are installed:

- `have_tables`: Boolean flag indicating whether the `tables` package is installed.
- `have_h5py`: Boolean flag indicating whether the `h5py` package is installed.
- `have_matplotlib`: Boolean flag indicating whether the `matplotlib` package is installed.
- `have_pylops`: Boolean flag indicating whether the `pylops` package is installed.
- `have_sounddevice`: Boolean flag indicating whether the `sounddevice` package is installed.
- `have_traitsui`: Boolean flag indicating whether the `traitsui` package is installed.

Example:

```python
if acoular.config.have_matplotlib:
    import matplotlib.pylab as plt
```

By configuring these options, you can customize the behavior of the Acoular library to suit your specific needs.
