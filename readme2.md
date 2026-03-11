In fig3d.ipynb, it is a Tidy3D script to measure the resonances of twisted bilayer photonic crystal slab. I have a Lumerical FDTD file that has been successfully reproduced for the article I want to repeat. I have ensured that the geometry structure of fig3d.ipynb is exactly the same as the setup in Lumerical FDTD. However, the resonances I measured are not clear, while the original paper's resonances have higher quality factors, and the resonance frequency differs from the original. For this, I think the possible reason for the error is that I only placed a single point dipole source at the center, and the electric field polarization direction was only Ey.

Now, I want to generate several point dipole sources with random positions within the range (1um, 1um, Gap+2*z_span) around the center, and also with random polarization directions. Also generate multiple FieldTimeMonitor within the same spatial range for monitoring if necessary. Of course, this may mean that when accumulating signals, the Ex, Ey, Ez signals from each monitor need to be summed up and then do the Fourier transform.

Add another field monitor in the x=0 plane

Is my idea reasonable? Please give your opinion. Please do not modify the original fig3d.ipynb, but generate a new.ipynb instead.

```
# setting apodization width (as a fraction of total simulation time) and apodization # center the center location of the apodization filter, employed for obtaining spectrum. apod_width = 0.125; apod_center = 0.5; # loop over each time monitor for(j=1:n_monitors) { Ex = pinch(getresult('m'+num2str(j),'Ex')); Ey = pinch(getresult('m'+num2str(j),'Ey')); Ez = pinch(getresult('m'+num2str(j),'Ez')); Etotal = Etotal + Ex + Ey + Ez; signal = Etotal; signal = signal*exp( - 0.5*(t-max(t)*apod_center)^2/(apod_width*max(t))^2); f = linspace(f1,f2,5000); fd = fd + abs(czt(signal,t,2*pi*f))^2; }
```