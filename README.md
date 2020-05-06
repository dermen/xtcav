## Description of data files

Run number is in the file name. 

```
>>> h5 = h5py.File("run175_xtcav.h5", "r")
>>> h5.keys()

[u'ebeam',
 u'event_time',
 u'evr',
 u'fiducials',
 u'first_pulse_maxpos',
 u'first_pulse_power',
 u'first_pulse_time',
 u'gas_detector',
 u'is_separated',
 u'phase_cav',
 u'power_trace',
 u'pulse_separation',
 u'pulse_t',
 u'second_pulse_maxpos',
 u'second_pulse_power',
 u'second_pulse_time',
 u'smoothed_power_trace']
```

### 'is\_separated'
1 means the XTCAV data had two peaks. 0 means there was only one primary peak found. -1 means something funky goin on (no maxima, or missing event data)

### 'ebeam'
event X-ray energy estimate provided by LCLS
### 'event_time'
time of the XTC event, provided by LCLS. If you want to use this value to sync up to other runs (with e.g. seconds and nanoseconds) , you can do something like  

```
In [2]: import h5py

In [3]: import psana

In [4]: f = h5py.File("run175_xtcav.h5", "r")

In [5]: et = f["event_time"][0]

In [8]: fid = f['fiducials'][0]

In [9]: et = f["event_time"][0]

In [11]: psana_et =psana.EventTime(et, fid)

In [12]: psana_et.nanoseconds()
Out[12]: 246439687

In [13]: psana_et.seconds()
Out[13]: 1537070201
```

also see [this](https://confluence.slac.stanford.edu/display/PSDM/Jump+Quickly+to+Events+Using+Timestamps).

### 'evr'
event code information (see log book)
### 'fiducials',
event fiducial for use with event time
### 'first\_pulse_maxpos'
position of the peak power in the first pulse (pixel units)
### 'first\_pulse_power'
peak power of the first pulse
### 'first\_pulse_time'
position of the peak power in the first pulse (femtosecond units)
### 'gas\_detector'
proportional to total intensity I believe

### 'phase\_cav'
cant remember
### 'power\_trace',
Y-axis of XTCAV trace
### 'pulse\_separation'
separation between two main pulses in femtoseconds
### 'pulse\_t'
x-axis of the XTCAV trace (femtoseconds I think). Probably the same for all events in a run, but not necessarilly for all runs
### 'second\_pulse_maxpos'
same as above but the rightmost peak
### 'second\_pulse_power'
same as above but the rightmost peak
### 'second\_pulse_time'
same as above, but rightmost peak
### 'smoothed\_power_trace'
moving average of the power trace


## Notes
if only 1 peak was found in the events xtcav trace, its information (peak height and arrival time) will be in the ```first_pulse*``` dataset, while ```second_pulse*``` dataset will have -1 values for that event.

You can start by fitting Gaussians to the curves in ```X=pulse_t, Y=power_trace```

