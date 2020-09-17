cp -r pgplot5.2.tar.gz /usr/local/src/
export http_proxy=http://proxy.pi.sjtu.edu.cn:3004/
export https_proxy=http://proxy.pi.sjtu.edu.cn:3004/
 yum -y install vim wget
yum -y groupinstall "Development Tools" 
yum -y install libpng12-devel
cd /usr/local
mkdir astrosoft
cd astrosoft
git clone https://github.com/scottransom/presto.git

cd /usr/local/src
wget http://www.fftw.org/fftw-3.3.5.tar.gz
tar xvf fftw-3.3.5.tar.gz
cd fftw-3.3.5
./configure --enable-shared --enable-single --prefix=/usr/local/astrosoft/fftw
make -j && make install

#Install PGPLOT
cd /usr/local/src
tar xvf pgplot5.2.tar.gz
cd /usr/local/astrosoft
mkdir pgplot
cd /usr/local/astrosoft/pgplot/
cp /usr/local/src/pgplot/drivers.list .
sed -i '19s/!/ /g' drivers.list
sed -i '20s/!/ /g' drivers.list
sed -i '38s/!/ /g' drivers.list
sed -i '44s/!/ /g' drivers.list
sed -i '45s/!/ /g' drivers.list
sed -i '46s/!/ /g' drivers.list
sed -i '47s/!/ /g' drivers.list
sed -i '71s/!/ /g' drivers.list
sed -i '72s/!/ /g' drivers.list
yum  -y install libX11 libX11-devel
yum -y install xorg-x11-drivers.x86_64
cd /usr/local/src/pgplot/src
cp grpckg1.inc grpckg1.inc.backup
cp pgplot.inc pgplot.inc.backup
sed -i '29s/8/32/g' grpckg1.inc
sed -i '7s/7/32/g' pgplot.inc
cd /usr/local/astrosoft/pgplot/
/usr/local/src/pgplot/makemake /usr/local/src/pgplot linux g77_gcc
sed -i '25s/77/fortran/g' makefile
sed -i '26s/FFLAGC=-u -Wall -fPIC -O/FFLAGC=-ffixed-form -ffixed-line-length-none -u -Wall -fPIC -O/g' makefile
sed -i '26s/1/FFLAGC=-ffixed-form -ffixed-line-length-none -u -Wall -fPIC -O/g' makefile
sed -i '29c LINUXWSLIBS   = -L/usr/include/X11/lib -lxview -lolgx -lX11 -lm -lf2c -lgfortran' makefile
make
make cpg

#Install GLIB
yum -y install glib2-devel 

#Install CFITSIO
cd /usr/local/src
wget  https://heasarc.gsfc.nasa.gov/FTP/software/fitsio/c/cfitsio3390.tar.gz
tar xvf cfitsio3390.tar.gz
cd cfitsio
./configure --prefix=/usr/local/astrosoft/cfitsio
make -j && make install

#Install TEMPO
cd /usr/local/src
git clone http://git.code.sf.net/p/tempo/tempo
cd tempo
yum -y install csh
./prepare
./configure --prefix=/usr/local/astrosoft/tempo
make -j && make install
cd /usr/local/astrosoft/tempo
mkdir src
cp /usr/local/src/tempo/tempo.hlp src/
cp /usr/local/src/tempo/tempo.cfg src/

#Install zlib and libpng
cd /usr/local/src
wget https://www.zlib.net/fossils/zlib-1.2.11.tar.gz
tar xvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
 ./configure --prefix=/usr/local/astrosoft/zlib
 make -j 64 && make install
 cd ..
wget https://iweb.dl.sourceforge.net/project/libpng/libpng16/1.6.36/libpng-1.6.36.tar.gz
tar xvf libpng-1.6.36.tar.gz
 cd libpng-1.6.36
 ./configure --prefix=/usr/local/astrosoft/libpng --with-zlib-prefix=/usr/local/astrosoft/zlib LDFLAGS="-L/usr/local/astrosoft/zlib/lib -lz"
 make -j 64 && make install

cd /usr/local/astrosoft
echo "export http_proxy=http://proxy.pi.sjtu.edu.cn:3004/" >> presto.sh
echo "export https_proxy=http://proxy.pi.sjtu.edu.cn:3004/" >> presto.sh
echo "export PATH=\$PATH:/usr/local/bin:/usr/local/astrosoft/presto/bin:/usr/local/astrosoft/pgplot:/usr/local/astrosoft/presto/bin:/nfshome/mcc/pfits:/usr/local/astrosoft/optimus:/usr/local/astrosoft/fv:/usr/local/astrosoft/psrcat_tar:/usr/local/astrosoft/tempo/src:/usr/local/astrosoft/tempo/bin:/usr/local/astrosoft/libpng/bin"  >> presto.sh
echo "export LD_LIBRARY_PATH=\$LD_LIBRARY_PATH:/usr/local/astrosoft/presto/lib:/usr/local/astrosoft/pgplot:/usr/local/astrosoft/fftw/lib:/usr/local/astrosoft/cfitsio/lib:/usr/local/astrosoft/libpng/lib" >> presto.sh
echo "export C_INCLUDE_PATH=\$C_INCLUDE_PATH:/usr/local/astrosoft/presto/include:/usr/local/astrosoft/cfitsio/include" >> presto.sh
echo "export PKG_CONFIG_PATH=/usr/local/astrosoft/cfitsio/lib/pkgconfig:/usr/local/astrosoft/fftw/lib/pkgconfig:/usr/local/astrosoft/libpng/lib/pkgconfig" >> presto.sh
echo "export PYTHONPATH=/usr/local/astrosoft/presto/python" >> presto.sh
echo "export PGPLOT_DIR=/usr/local/astrosoft/pgplot" >> presto.sh
echo "export PGPLOT_FONT=/usr/local/astrosoft/pgplot/grfont.dat" >> presto.sh
echo "export PGPLOT_DEV=/xwine" >> presto.sh
echo "export PGPLOT_LIB=\"-L /usr/X11R6/lib -lX11 -L /usr/local/astrosoft/pgplot -lpgplot\"" >> presto.sh
echo "export PRESTO=/usr/local/astrosoft/presto" >> presto.sh
echo "export TEMPO=/usr/local/astrosoft/tempo/src" >> presto.sh
echo "export PSRCAT_FILE=/usr/local/astrosoft/psrcat_tar/psrcat.db" >> presto.sh
source /usr/local/astrosoft/presto.sh

#Install presto
cd /usr/local/astrosoft/presto/src
make makewisdom 
make prep 
make
cd /usr/local/src

#Install pip,numpy,scipy
wget https://files.pythonhosted.org/packages/c2/f7/c7b501b783e5a74cf1768bc174ee4fb0a8a6ee5af6afa92274ff964703e0/setuptools-40.8.0.zip
wget https://files.pythonhosted.org/packages/4c/4d/88bc9413da11702cbbace3ccc51350ae099bb351febae8acc85fec34f9af/pip-19.0.2.tar.gz
unzip setuptools-40.8.0.zip
cd setuptools-40.8.0
python setup.py install
cd ..
tar xvf pip-19.0.2.tar.gz
cd pip-19.0.2
python setup.py install
pip install numpy scipy

cd /usr/local/astrosoft/presto
sed -i '21c ppgplot_libraries = ["gfortran","cpgplot", "pgplot", "X11", "png", "m"]' setup.py
cd /usr/local/astrosoft/presto/src
make 
cd /usr/local/astrosoft/presto
export  PYTHONPATH=
cd /usr/local/astrosoft
sed -i '7c export PYTHONPATH=' presto.sh
cd /usr/local/astrosoft/presto
yum -y install python-devel fftw-devel libpng-devel
pip install .
yum -y install tkinter latex2html
yum -y install tk-devel tcl-devel
