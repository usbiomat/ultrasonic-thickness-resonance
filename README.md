ultrasonic-thickness-resonance
==============================

Program to estimate material properties from the measured first order thickness resonance in the ultrasonic transmission coefficient of a homogeneous plate at normal incidence

The following material properties are obtained: 
    1. Plate density
    2. Plate thickness
    3. Ultrasound velocity in the plate
    4. Ultrasound attenuation coefficient at resonant frequency in the plate
    5. Variation of the attenuation coefficient with the frequency assuming a power law.

DEPENDENCIES:
    
    NUMPY
    PYLAB (only if graphic display is enabled)

DEFINITIONS: 
    
    Measurement: FFT of the signal transmitted through the sample (magnitude -dB- and phase -rad-)
    Calibration: FFT of the signal transmitted from transmitter to receiver without the sample between them
    (amplitude -dB- and phase -rad-).

INPUT: 
  
  An ascii file with three columns (floats),
  1st col.: Frequency (in Hz),
  2nd col.: Amplitude spectrum of the measurement (in dB)
            - the amplitude spectrum of the calibration (in dB).
  3rd col.: Phase spectrum of the measurement (in rad).
              - the phse spectrum of the calibration (in rad).

  This file must be in: c:\Python27\Scripts\medida.dat
  
  The default path is the one shown before: c:\Python27\Scripts\
  Alternatively, it is possible to re-define the path to allocate input and output files in a user defined path.
  This can be done by storing the user defined path in the file:
      
      c:\Python27\Scripts\path_CT.txt
      
   This file must contain one line of text with the desired path
     
     (for example: c:\Users\JS\Dropbox\Python\  )
     
    If the file:  \medida.dat is no found, then he program ask for the file name, including extension, (raw_input)
     
ADVANCED IN/OUT DATA:
    
   Other input data can be provided by creating the file: 'input_python14.txt' in c:\Python27\Scripts\ 
   or in the user defined path.
   The format of this file is one row of floats space separated. Example:
        1.2000000e+000  3.4300000e+002  2.0000000e+001  4.0000000e+002  1.0000000e-002 9.9999e-001 1.0000000e+000
    These data correspond to:
        1. Density of the fluid where the plate is located (air at 20 C is the default case: 1.2 kg/m3)
        2. Ultrasound velocity in this fluid (343m/s in the default case)
        3. Loss in dB mesured from the resonance peak to limit the analysis to this frequency band (Default value: 10 dB)
        4. Maximum number of steps allowed in the Gradient Descent (400 in the default case)
        5. Step size in the Gradient Descent, constant (0.01 in the default case)
        6. Minimum increment of the error allowed in the Gradient Descent, if no, the routine is interrupted and values returned
           (0.99999 in the default case).
        7. Option for graphical display 1: on, 0: off. If enabled, the the program shows plots with transmission
           coefficient spectra, both, claculated and measured. This option requires PYLAB
        

OUTPUT FILES: 
    
    (path)...\output.dat
        A file with theoretically calculated transmission coefficient spectra (with same format as the input data):
        [frequency (Hz) Magnitude spectrum (dB) Phase spectrum (Hz)]
    
    (path)...\output2.dat
        A file with 3 rows and 6 columns:
            
        [1:plate thickness (m); 2:ultrasound velocity(m/s); 3:attenuation coeff./Np/m); 4.plate density (kg/m3); 5.exponent in the attenuation vs freq power law; 6.resonant frequency (Hz)]
        [1:IEE Magnitude spectrum; 2: IEE Phase Spectrum; 3. FEE Magnitude spectrum; 4. FEE Phase spectrum; 5. Number of steps in the GD routine; 6. Loss from peak for bandwidth (dB)]
        [1:Q-factor of the resonance; 2:Maximum of the Magnitude spectrum (dB); 3:Running time (s) ; 4: Maximum number of steps allowed in GD routine; 5: Step size in GD routine; 6: Minimum error improvement for another step in GD]
        
        IEE: Initial error estiamtion
        FEE: Final error estimation

CALCULATION METHOD:
    
    Initial values are obtained from the resonant frequency, the amplitude and the phase at resonance,
    and the Q factor of the resonance. The layer parameters are modified to improve the fitting of the
    theoretically predicted spectra into the experimentally measured one. The minimization of the error
    is performed by using a Gradient Descent algorithm.


Author: Tomas Gómez Alvarez-Arenas
        ITEFI-CSIC
        www.us-biomat.com
        t.gomez@csic.es
        03/25/2014

Starting point is CT12_GD.py
 
 """
