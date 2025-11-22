# EXP 3 : IIR-BUTTERWORTH-FITER-DESIGN

## AIM: 

 To design an IIR Butterworth filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```
clc;
close(winsid()); // Close all graphics windows

// Input parameters
wp = input("Enter the pass band frequency (Radians) = ");
ws = input("Enter the stop band frequency (Radians) = ");
alphap = input("Enter the pass band attenuation (dB) = ");
alphas = input("Enter the stop band attenuation (dB) = ");
T = input("Enter the value of sampling time = ");

// Pre-warping using Bilinear Transformation
omegap = (2/T) * tan(wp/2);
disp(omegap, "omegap =");

omegas = (2/T) * tan(ws/2);
disp(omegas, "omegas =");

// Order of the filter
N = log10(((10^(0.1*alphas))-1) / ((10^(0.1*alphap))-1)) / (2*log10(omegas/omegap));
disp(N, "N (calculated) =");

N = ceil(N); // Round off
disp(N, "Round off value of N =");

// Cutoff frequency
omegac = omegap / (((10^(0.1*alphap)) - 1)^(1/(2*N)));
disp(omegac, "omegac =");

// Normalised Analog LPF
disp("Normalised Analog LPF Transfer function H(s) =");
hs_Normalised = analpf(N, "butt", [0,0], 1);
disp(hs_Normalised);

// Actual Analog LPF
disp("Analog LPF Transfer function H(s) =");
hs = analpf(N, "butt", [0,0], omegac);
disp(hs);

// Bilinear transformation
z = poly(0, "z"); // Define variable z
Hz = horner(hs, (2/T)*((z - 1)/(z + 1)));
disp("Digital LPF Transfer function H(z) =");
disp(Hz);

// Frequency response
HW = frmag(Hz, 512); 
w = 0:%pi/511:%pi;
plot(w/%pi, abs(HW));
xlabel("Normalized Digital Frequency (w/π)");
ylabel("Magnitude");
title("Frequency Response of Butterworth IIR LPF");
```
## CONSOLE WINDOW(LPF)
```

Enter the pass band frequency (Radians) = 0.2*%pi

Enter the stop band frequency (Radians) = 0.6*%pi

Enter the pass band attenuation (dB) = 2

Enter the stop band attenuation (dB) = 14

Enter the value of sampling time = 1


   0.6498394

  "omegap ="

   2.7527638

  "omegas ="

   1.2881785

  "N (calculated) ="

   2.

  "Round off value of N ="

   0.7430823

  "omegac ="

  "Normalised Analog LPF Transfer function H(s) ="

           1           
   ------------------  
   1 +1.4142136s +s^2  

  "Analog LPF Transfer function H(s) ="

           0.5521712          
   -------------------------  
   0.5521712 +1.050877s +s^2  

  "Digital LPF Transfer function H(z) ="

   0.5521712 +1.1043425z +0.5521712z^2  
   -----------------------------------  
   2.4504172 -6.8956575z +6.6539253z^2  
```

## PROGRAM (HPF): 
```
clc;
close;
wp = input('Enter the pass band frequency (Radians )= ');
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the Value of sampling Time= ');

// Pre-warping (Bilinear Transformation)
omegap = (2/T) * tan(wp/2);
disp(omegap, 'omegap=');
omegas = (2/T) * tan(ws/2);
disp(omegas, 'omegas=');

// Order of the filter
N = log10(((10^(0.1*alphas))-1) / ((10^(0.1*alphap))-1)) / (2*log10(omegas/omegap));
disp(N,'N=');
N = ceil(N);
disp(N,'Round off value of N=');

// Cut off frequency
omegac = omegap / (((10^(0.1*alphap))-1)^(1/(2*N)));
disp(omegac,'omegac=');

// Normalised Analog LPF Transfer function
disp('Normalised Analog LPF Transfer function H(S)=');
hs_Normalised = analpf(N,'butt',[0,0],1);
disp(hs_Normalised);

// Analog LPF Transfer function
disp('Analog LPF Transfer function H(S)=');
hs = analpf(N,'butt',[0,0],omegac);
disp(hs);

s = poly(0,'s');
hpf_s = horner(hs, omegac/s);   // substitute s → omegac/s
disp('Analog HPF Transfer function H(S)=');
disp(hpf_s);

// Bilinear Transformation to Digital
z = poly(0,'z'); // Defining variable z
Hz = horner(hpf_s,(2/T)*((z-1)/(z+1))); 
disp('Digital HPF Transfer function H(Z)=');
disp(Hz);

// Frequency Response
HW = frmag(Hz,512);
w = 0:%pi/511:%pi;
plot(w/%pi, abs(HW));

xlabel(' Normalized Digital Frequency w');
ylabel('Magnitude');
title(' Frequency Response of Butterworth IIR HPF');
```
## CONSOLE WINDOW(HPF)
```

Enter the pass band frequency (Radians )= 0.4*%pi

Enter the stop band frequency (Radians )= 0.6*%pi

Enter the pass band attenuation (dB)= 3

Enter the stop band attenuation (dB)= 20

Enter the Value of sampling Time= 1


   1.4530851

  "omegap="

   2.7527638

  "omegas="

   3.5997416

  "N="

   4.

  "Round off value of N="

   1.4539479

  "omegac="

  "Normalised Analog LPF Transfer function H(S)="

         1         
   --------------  
   1 +2.6131259s   
    +3.4142136s^2  
    +2.6131259s^3  
    +s^4           

  "Analog LPF Transfer function H(S)="

     4.4688458     
   --------------  
   4.4688458       
    +8.0316886s    
    +7.2175261s^2  
    +3.7993489s^3  
    +s^4           

  "Analog HPF Transfer function H(S)="

    4.4688458s^4   
   --------------  
   4.4688458       
    +11.677657s    
    +15.257594s^2  
    +11.677657s^3  
    +4.4688458s^4  

  "Digital HPF Transfer function H(Z)="

   0.2817491       
    -1.1269964z    
    +1.6904946z^2  
    -1.1269964z^3  
    +0.2817491z^4  
   --------------  
   0.0796926       
    -0.5043747z    
    +1.3151747z^2  
    -1.6087435z^3  
    +z^4           
exec: Wrong number of output argument(s): 0 expected.

  
```


## OUTPUT (LPF) : 
<img width="1915" height="961" alt="Screenshot 2025-09-22 152346" src="https://github.com/user-attachments/assets/00bdcb3f-3d81-495d-a8c9-7414e9d526db" />



## OUTPUT (HPF) : 
<img width="1919" height="958" alt="Screenshot 2025-09-22 153932" src="https://github.com/user-attachments/assets/d4da133c-89e6-43c6-9974-7c7370551e75" />


## RESULT: 
Thus,the Butterworth using LPF and HPF is verified
