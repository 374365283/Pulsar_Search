source /root/presto.sh
export PRESTO=/usr/local/astrosoft/presto
#readfile GBT_Lband_PSR.fil 
time1='date +%s'
rfifind -time 2.0 -o Lband GBT_Lband_PSR.fil 
time2='date +%s'
rfifind -time 1.0 -o Lband GBT_Lband_PSR.fil 
time3='date +%s'
rfifind -nocompute -time 1.0 -freqsig 6.0 -mask Lband_rfifind.mask -o Lband GBT_Lband_PSR.fil 
time4='date +%s'
prepdata -nobary -o Lband_topo_DM0.00 -dm 0.0 -mask Lband_rfifind.mask -numout 530000 GBT_Lband_PSR.fil 
time5='date +%s'
#exploredat Lband_topo_DM0.00.dat 
realfft Lband_topo_DM0.00.dat 
time6='date +%s'
#explorefft Lband_topo_DM0.00.fft 
accelsearch -numharm 4 -zmax 0 Lband_topo_DM0.00.dat 
time7='date +%s'
#less -S Lband_topo_DM0.00_ACCEL_0
#jed Lband.birds
cp Lband_rfifind.inf Lband.inf
time8='date +%s'
makezaplist.py Lband.birds
time9='date +%s'
#explorefft Lband_topo_DM0.00.fft
prepfold -p 1.0 GBT_Lband_PSR.fil
time10='date +%s'
DDplan.py -d 500.0 -n 96 -b 96 -t 0.000072 -f 1400.0 -s 32 -r 0.5
time11='date +%s'
prepsubband -nsub 32 -lodm 0.0 -dmstep 2.0 -numdms 24 -numout 132500 -downsamp 4 -mask Lband_rfifind.mask -o Lband GBT_Lband_PSR.fil 
time12='date +%s'
cp $PRESTO/tests/dedisp.py .
#jed dedisp.py 
time13='date +%s'
python dedisp.py
time14='date +%s'
mkdir subbands
mv *.sub* subbands/
rm -rf Lband*topo*
time15='date +%s'
ls *.dat | xargs -n 1 realfft
time16='date +%s'
ls *.fft | xargs -n 1 zapbirds -zap -zapfile Lband.zaplist -baryv -5.69726e-05
time17='date +%s'
ls *dat | xargs -n 1 accelsearch -zmax 0
time18='date +%s'
cp $PRESTO/examplescripts/ACCEL_sift.py .
time19='date +%s'
python ACCEL_sift.py > cands.txt
time20='date +%s'
#less cands.txt 
prepfold -accelcand 2 -accelfile Lband_DM62.00_ACCEL_0.cand Lband_DM62.00.dat
time21='date +%s'
#ls subbands/
prepfold -accelcand 2 -accelfile Lband_DM62.00_ACCEL_0.cand -dm 62 subbands/Lband_DM72.00.sub??
time22='date +%s'
prepfold -n 64 -nsub 96 -p 0.004621638 -dm 62.0 GBT_Lband_PSR.fil 
time23='date +%s'
single_pulse_search.py *dat
time24='date +%s'
#gv Lband_singlepulse.ps

duration=`echo "$time2-$time1" | bc -l`
echo "rfifind1 = $duration sec"
duration=`echo "$time3-$time2" | bc -l`
echo "rfifind2 = $duration sec"
duration=`echo "$time4-$time3" | bc -l`
echo "rfifind3 = $duration sec"
duration=`echo "$time5-$time4" | bc -l`
echo "preparedata = $duration sec"
duration=`echo "$time6-$time5" | bc -l`
echo "realfft = $duration sec"
duration=`echo "$time7-$time6" | bc -l`
echo "accelsearch = $duration sec"
duration=`echo "$time9-$time8" | bc -l`
echo "makezaplist = $duration sec"
duration=`echo "$time10-$time9" | bc -l`
echo "prepfold = $duration sec"
duration=`echo "$time11-$time10" | bc -l`
echo "DDplan = $duration sec"
duration=`echo "$time12-$time11" | bc -l`
echo "prepsubband = $duration sec"
duration=`echo "$time14-$time13" | bc -l`
echo "dedisp = $duration sec"
duration=`echo "$time16-$time15" | bc -l`
echo "realfft = $duration sec"
duration=`echo "$time17-$time16" | bc -l`
echo "zapbirds = $duration sec"
duration=`echo "$time18-$time17" | bc -l`
echo "accelsearch = $duration sec"
duration=`echo "$time20-$time19" | bc -l`
echo "ACCEL_sift = $duration sec"
duration=`echo "$time21-$time20" | bc -l`
echo "prepfold = $duration sec"
duration=`echo "$time22-$time21" | bc -l`
echo "prepfold = $duration sec"
duration=`echo "$time23-$time22" | bc -l`
echo "prepfold = $duration sec"
duration=`echo "$time24-$time23" | bc -l`
echo "single_pulse_search = $duration sec"

