

### Esboço de uma pesquisa PRESTO

Precisamos fazer as seguintes analises

1) Examine o formato dos dados ```readfile```
2) Pesquise RFI ```rfifind```
3) Faça uma série temporal topocêntrica DM = 0 ```prepdata e exploredat```
4) FFT a série temporal ```realfft```
5) Identifique “passarinhos” para zapear nas pesquisas ```explorefft e accelsearch```
6) Faça zaplist ``` makezaplist.py ```
7) Faça um plano de desdispersão ```DDplan.py```
8) Desdisperso ```prepsubband```
9) Pesquise os dados por sinais periódicos ```accelsearch```
10) Pesquise os dados por pulsos únicos ```single_pulse_search.py```
11) Analise os candidatos ```ACCEL_sift.py```
12) Dobre os melhores candidatos ```prepfold```
13) Comece a cronometrar o novo pulsar ```prepfold e get_TOAs.py```

Baixe o arquivo 

```Ruby
wget http://www.cv.nrao.edu/~sransom/GBT_Lband_PSR.fil
```

Execute no seu bash 

```Ruby
readfile GBT_Lband_PSR.fil
```

Você encontrará algo do tipo

```Ruby
1: From the SIGPROC filterbank file 'GBT_Lband_PSR.fil':
                  Telescope = GBT
                Source Name = Mystery_PSR
            Obs Date String = 2004-01-06T11:38:09
             MJD start time = 53010.48482638889254
                   RA J2000 = 16:43:38.1000
             RA J2000 (deg) = 250.90875        
                  Dec J2000 = -12:24:58.7000
            Dec J2000 (deg) = -12.4163055555556
                  Tracking? = True
              Azimuth (deg) = 0
           Zenith Ang (deg) = 0
            Number of polns = 2 (summed)
           Sample time (us) = 72               
         Central freq (MHz) = 1400             
          Low channel (MHz) = 1352.5           
         High channel (MHz) = 1447.5           
        Channel width (MHz) = 1                
         Number of channels = 96
      Total Bandwidth (MHz) = 96               
                       Beam = 1 of 1
            Beam FWHM (deg) = 0.147
         Spectra per subint = 2400
           Spectra per file = 531000
      Time per subint (sec) = 0.1728
        Time per file (sec) = 38.232
            bits per sample = 4
          bytes per spectra = 48
        samples per spectra = 96
           bytes per subint = 115200
         samples per subint = 230400
                zero offset = 0                
           Invert the band? = False
       bytes in file header = 365

```

```Ruby
rfifind -time 2.0 -o Lband GBT_Lband_PSR.fil
```

retornará para você algo do tipo

```Ruby
Assuming the data are SIGPROC filterbank format...
Reading SIGPROC filterbank data from 1 file:
  'GBT_Lband_PSR.fil'

    Number of files = 1
       Num of polns = 2 (summed)
  Center freq (MHz) = 1400
    Num of channels = 96
    Sample time (s) = 7.2e-05       
     Spectra/subint = 2400
   Total points (N) = 531000
     Total time (s) = 38.232        
     Clipping sigma = 6.000
   Invert the band? = False
          Byteswap? = False
     Remove zeroDM? = False

File  Start Spec   Samples     Padding        Start MJD
----  ----------  ----------  ----------  --------------------
1              0      531000           0  53010.48482638889254

Analyzing data sections of length 28800 points (2.0736 sec).
  Prime factors are:  2 2 2 2 2 2 2 3 3 5 5 

Writing mask data  to 'Lband_rfifind.mask'.
Writing  RFI data  to 'Lband_rfifind.rfi'.
Writing statistics to 'Lband_rfifind.stats'.

Massaging the data ...

Amount Complete = 100%
There are 31 RFI instances.

Total number of intervals in the data:  1824

  Number of padded intervals:       96  ( 5.263%)
  Number of  good  intervals:     1487  (81.524%)
  Number of  bad   intervals:      241  (13.213%)

  Ten most significant birdies:
#  Sigma     Period(ms)      Freq(Hz)       Number 
----------------------------------------------------
1  6.83      11.5521         86.5644        147     
2  6.71      11.6494         85.841         170     
3  6.68      11.6168         86.0822        146     
4  6.57      8.76787         114.053        1       
5  6.53      11.5844         86.3233        145     
6  6.10      11.52           86.8055        135     
7  5.96      11.4881         87.0467        107     
8  5.89      11.7153         85.3588        21      
9  5.88      11.6823         85.5999        23      
10 5.65      11.7484         85.1177        24      

  Ten most numerous birdies:
#  Number    Period(ms)      Freq(Hz)       Sigma 
----------------------------------------------------
1  493       34.56           28.9352        4.82    
2  351       34.8504         28.6941        4.75    
3  280       17.28           57.8704        4.85    
4  271       17.3523         57.6292        4.80    
5  180       17.4252         57.3881        4.68    
6  179       17.4987         57.147         4.67    
7  170       11.6494         85.841         6.71    
8  147       11.5521         86.5644        6.83    
9  146       11.6168         86.0822        6.68    
10 145       11.5844         86.3233        6.53    

Done.
```
Digite no terminal 

```Ruby
prepdata -nobary -o Lband_topo_DM0.00 \-dm 0.0 -mask Lband_rfifind.mask \GBT_Lband_PSR.fil
```

Você receberá algo do tipo 

```Ruby
Assuming the data are SIGPROC filterbank format...
Reading SIGPROC filterbank data from 1 file:
  'GBT_Lband_PSR.fil'

    Number of files = 1
       Num of polns = 2 (summed)
  Center freq (MHz) = 1400
    Num of channels = 96
    Sample time (s) = 7.2e-05       
     Spectra/subint = 2400
   Total points (N) = 531000
     Total time (s) = 38.232        
     Clipping sigma = 6.000
   Invert the band? = False
          Byteswap? = False
     Remove zeroDM? = False

File  Start Spec   Samples     Padding        Start MJD
----  ----------  ----------  ----------  --------------------
1              0      531000           0  53010.48482638889254

Setting a 'good' output length of 537600 samples
Read mask information from 'Lband_rfifind.mask'

Attempting to read the data statistics from 'Lband_rfifind.stats'...
...succeded.  Set the padding values equal to the mid-80% channel averages.
Writing output data to 'Lband_topo_DM0.00.dat'.
Writing information to 'Lband_topo_DM0.00.inf'.

Massaging the data ...

Amount Complete =  99%

Done.
Simple statistics of the output data:
             Data points written:  530400
          Padding points written:  7200
           Maximum value of data:  898.83
           Minimum value of data:  683.15
              Data average value:  785.54
         Data standard deviation:  21.64

```
Digite no terminal 

```Ruby
exploredat Lband_topo_DM0.00.dat
```
Retornarar algo do tipo 

```Ruby
     Interactive Data Explorer
         by Scott M. Ransom
            November, 2001

 Button or Key            Effect
 -------------            ------
 Left Mouse or I or A     Zoom in  by a factor of 2
 Right Mouse or O or X    Zoom out by a factor of 2
 <                        Shift left  by a full screen width
 >                        Shift right by a full screen width
 ,                        Shift left  by 1/8 of the screen width
 .                        Shift right by 1/8 of the screen width
 +/_                      Increase/Decrease the top edge
 =/-                      Increase/Decrease the bottom edge
 SPACE                    Toggle statistics and sample plotting on/off
 M                        Toggle between median and average
 S                        Scale the powers automatically
 V                        Print the statistics for the current view
 P                        Print the current plot to a file
 G                        Go to a specified time
 ?                        Show this help screen
 Q                        Quit

Examining Mystery_PSR data from 'Lband_topo_DM0.00.dat'.

PGPLOT /xw: cannot connect to X server []

```
Observe que esse erro é devido a interação do pgplot e como está dentro do docker não consegue plotar o grafico, logo, não aparecerá o grafico, mas caso seja instalação fora do docker você conseguirá verificar o grafico plotado.

Digite no terminal 

```Ruby
realfft Lband_topo_DM0.00.dat
```
retornaremos a 

```Ruby
Real-Valued Data FFT Program v3.0
        by Scott M. Ransom

   1:  Processing data in 'Lband_topo_DM0.00.dat'

Data OK.  There are 537600 floats.

   1:   Result will be in '/software/presto5/Lband_topo_DM0.00.fft'

Performing in-core forward FFT on data:
   Reading.
   Transforming.
   Writing.
Finished.

Timing summary:
  CPU usage: 0.010 sec total (0.010 sec user, 0.000 sec system)
  Total time elapsed:  0.020 sec (0.020 sec/file)

```
Digite no terminal 

```Ruby
explorefft Lband_topo_DM0.00.fft
```
retornaremos a 

```Ruby


      Interactive FFT Explorer
         by Scott M. Ransom
            October, 2001

 Button or Key       Effect
 -------------       ------
 Left Mouse or I     Zoom in  by a factor of 2
 Right Mouse or O    Zoom out by a factor of 2
 Middle Mouse or D   Show details about a selected frequency
 <                   Shift left  by a full screen width
 >                   Shift right by a full screen width
 ,                   Shift left  by 1/8 of the screen width
 .                   Shift right by 1/8 of the screen width
 + or =              Increase the power scale (make them taller)
 - or _              Decrease the power scale (make them shorter)
 S                   Scale the powers automatically
 N                   Re-normalize the nowers by one of several methods
 P                   Print the current plot to a file
 G                   Go to a specified frequency
 L                   Load a zaplist showing potential RFI locations
 Z                   Add a frequency chunk to the RFI zaplist
 H                   Show the harmonics of the center frequency
 ?                   Show this help screen
 Q                   Quit

Examining Mystery_PSR data from 'Lband_topo_DM0.00.fft'.

Memory mapping the input FFT.  This may take a while...
PGPLOT /xw: cannot connect to X server []

```

digite no terminal 

```Ruby
accelsearch -numharm 4 -zmax 0 \ Lband_topo_DM0.00.dat
```

```Ruby
    Fourier-Domain Acceleration and Jerk Search Routine
                    by Scott M. Ransom

Analyzing Mystery_PSR data from 'Lband_topo_DM0.00.dat'.

Reading and FFTing the time series...done.
Removing red-noise...done.

Normalizing powers using median-blocks (default).

Full f-fdot plane would need 0.00 GB: using in-memory accelsearch.

Searching with up to 4 harmonics summed:
  f = 1.0 to 6944.4 Hz
  r = 38.0 to 268799.0 Fourier bins
  z = 0.0 to 0.0 Fourier bins drifted

Generating correlation kernels:
  Harm  1/1 :     1 kernels,    0 < z < 0    (2048 pt FFTs)
Total RAM used by correlation kernels:  0.000 GB
Done generating kernels.

Starting the search.

Amount of search complete = 100%

Done searching.  Now optimizing each candidate.

Removed 18 likely harmonically related candidates.
Amount of optimization complete = 100%
width < len (7) in center_string(outstring, '1721.23', width=6)

width < len (11) in center_string(outstring, ' 0.4942(38)', width=10)

width < len (11) in center_string(outstring, ' 0.9943(59)', width=10)


Searched the following approx numbers of independent points:
  1 harmonic:      268761
  2 harmonics:     134380
  4 harmonics:      67190

Timing summary:
    CPU time: 0.120 sec (User: 0.120 sec, System: 0.000 sec)
  Total time: 0.120 sec

Final candidates in binary format are in 'Lband_topo_DM0.00_ACCEL_0.cand'.
Final Candidates in a text format are in 'Lband_topo_DM0.00_ACCEL_0'.

```
digite no terminal 

```Ruby
cp Lband_rfifind.inf Lband.inf
```
Digite no terminal 

```Ruby
makezaplist.py Lband.birds
```
caso de erro, de não encontrar, você deve encontrar onde está o repositorio, ao encontrar copie para o seu caso. 

```Ruby
cp ./tests/Lband.birds .
```

Digite no terminal 
```Ruby
zapbirds -zap -zapfile Lband.zaplist \Lband_topo_DM0.00.fft
```
Digite no terminal 
```Ruby
prepdata -o tmp GBT_Lband_PSR.fil | grep Average
```
Voce receberá como resposta
```Ruby
Average topocentric velocity (c) = -5.697334e-05
```
Digite no terminal 

```Ruby
DDplan.py -d 500.0 -n 96 -b 96 -t 0.000072 \-f 1400.0 -s 32 -r 0.5
```
no meu caso deu erro por pgplot soliciar ```device = "/xwin"```, por sugestão abra o nano e modifique o device

```Ruby
cd bin
nano DDplan.py
```
modifique o device para ```device='ffdot_combined.eps/VCPS'```

Digite no terminal 

```Ruby
prepsubband -nsub 32 -lodm 0.0 -dmstep 2.0 -numdms 24 -downsamp 4 -mask Lband_rfifind.mask -o Lband GBT_Lband_PSR.fil

```

retornará algo do tipo

```Ruby
Assuming the data are SIGPROC filterbank format...
Reading SIGPROC filterbank data from 1 file:
  'GBT_Lband_PSR.fil'

    Number of files = 1
       Num of polns = 2 (summed)
  Center freq (MHz) = 1400
    Num of channels = 96
    Sample time (s) = 7.2e-05       
     Spectra/subint = 2400
   Total points (N) = 531000
     Total time (s) = 38.232        
     Clipping sigma = 6.000
   Invert the band? = False
          Byteswap? = False
     Remove zeroDM? = False

File  Start Spec   Samples     Padding        Start MJD
----  ----------  ----------  ----------  --------------------
1              0      531000           0  53010.48482638889254

Read mask information from 'Lband_rfifind.mask'

Attempting to read the data statistics from 'Lband_rfifind.stats'...
...succeded.  Set the padding values equal to the mid-80% channel averages.
Writing output data to:
   'Lband_DM0.00.dat'
   'Lband_DM2.00.dat'
   'Lband_DM4.00.dat'
   'Lband_DM6.00.dat'
   'Lband_DM8.00.dat'
   'Lband_DM10.00.dat'
   'Lband_DM12.00.dat'
   'Lband_DM14.00.dat'
   'Lband_DM16.00.dat'
   'Lband_DM18.00.dat'
   'Lband_DM20.00.dat'
   'Lband_DM22.00.dat'
   'Lband_DM24.00.dat'
   'Lband_DM26.00.dat'
   'Lband_DM28.00.dat'
   'Lband_DM30.00.dat'
   'Lband_DM32.00.dat'
   'Lband_DM34.00.dat'
   'Lband_DM36.00.dat'
   'Lband_DM38.00.dat'
   'Lband_DM40.00.dat'
   'Lband_DM42.00.dat'
   'Lband_DM44.00.dat'
   'Lband_DM46.00.dat'
Setting a 'good' output length of 134400 samples

Generating barycentric corrections...
   Average topocentric velocity (c) = -5.697334e-05
   Maximum topocentric velocity (c) = -5.697064e-05
   Minimum topocentric velocity (c) = -5.697603e-05

De-dispersing and barycentering using:
       Subbands = 32
     Average DM = 23
     Downsample = 4
  New sample dt = 0.000288

Amount complete = 100%

Done.

Simple statistics of the output data:
             Data points written:  131407
          Padding points written:  2993
    Bins added for barycentering:  7
           Maximum value of data:  853.36
           Minimum value of data:  735.95
              Data average value:  785.55
         Data standard deviation:  12.33

```

Digite no terminal 

```Ruby
mkdir subbands
mv *.sub* subbands/
rm -f Lband*topo* tmp*
```
Use xargs (awesome Unix command) to fft and zap the *.dat files

```Ruby
ls *.dat | xargs -n 1 realfft
ls *.fft | xargs -n 1 zapbirds -zap \ -
zapfile Lband.zaplist -baryv -5.697334e-05
```
digite no terminal 

```Ruby
prepdata -o tmp GBT_Lband_PSR.fil | grep Average
```

Searching for Periodic Signals

```Ruby
accelsearch -zmax 0 Lband_DM0.00.fft
```

Sifting the periodic candidates

digite no terminal 

```Ruby
python ACCEL_sift.py > cands.txt
```
digite no terminal 

```Ruby
prepfold -accelcand 1 -accelfile \
Lband_DM62.00_ACCEL_0.cand Lband_DM62.00.dat
```

digite no terminal 

```Ruby
prepfold -n 64 -nsub 96 -p 0.004621638 -dm 62.0 \GBT_Lband_PSR.fil
```

Continua..

Falta apenas alumas partes.


