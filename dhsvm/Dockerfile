FROM ubuntu:bionic


RUN buildDeps='git liblapack-dev python3-dev build-essential flex ca-certificates libx11-dev' \
&& apt-get update \
&& apt-get install -y --no-install-recommends $buildDeps \
&& git clone -b glacier https://github.com/pnnl/DHSVM-PNNL.git /home/jovyan/libs/DHSVM-PNNL \
&& sed -i '/# CC = gcc/s/^# //' /home/jovyan/libs/DHSVM-PNNL/DHSVM/UFconfig/UFconfig.mk \
&& sed -i '/# CFLAGS = -O3 -fexceptions/s/^# //' /home/jovyan/libs/DHSVM-PNNL/DHSVM/UFconfig/UFconfig.mk \
&& sed -i '/# BLAS = -lgoto -lfrtbegin -lg2c $(XERBLA) -lpthread/s/^# //' /home/jovyan/libs/DHSVM-PNNL/DHSVM/UFconfig/UFconfig.mk \
&& sed -i 's/F77 = gfortran/# &/' /home/jovyan/libs/DHSVM-PNNL/DHSVM/UFconfig/UFconfig.mk \
&& sed -i 's/CFLAGS = -O3 -fno-common -no-cpp-precomp -fexception/# &/' /home/jovyan/libs/DHSVM-PNNL/DHSVM/UFconfig/UFconfig.mk \
&& sed -i 's/BLAS = -framework Accelerate/# &/' /home/jovyan/libs/DHSVM-PNNL/DHSVM/UFconfig/UFconfig.mk \
&& sed -i 's/LAPACK = -framework Accelerate/# &/' /home/jovyan/libs/DHSVM-PNNL/DHSVM/UFconfig/UFconfig.mk \
&& sed -i 's/DEFS =  -DHAVE_X11 -DHAVE_GLACIER/DEFS =  -DHAVE_X11/' /home/jovyan/libs/DHSVM-PNNL/DHSVM/sourcecode/makefile \
&& rm /home/jovyan/libs/DHSVM-PNNL/DHSVM/Lib/libcxsparse.a 

RUN cd /home/jovyan/libs/DHSVM-PNNL/DHSVM/Lib \
&& make 

RUN cd /home/jovyan/libs/DHSVM-PNNL/DHSVM/sourcecode \
&& make 

# Copy the built libraries to /usr/local/bin
RUN mv /home/jovyan/libs/DHSVM-PNNL/DHSVM/sourcecode/DHSVM3.1.3 /usr/local/bin 

## clean up
#rm -rf /home/jovyan/libs/DHSVM* 
#apt-get purge -y --auto-remove $buildDeps

