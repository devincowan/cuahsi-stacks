FROM wrfhydro/dev:base

ARG WRF_HYDRO_VERSION

USER docker

#ENV VERSION=v5.0.3

# get the WRF-Hydro code
RUN cd /tmp \
&& git clone https://github.com/NCAR/wrf_hydro_nwm_public.git \
&& cd wrf_hydro_nwm_public \
&& git checkout tags/$WRF_HYDRO_VERSION

# add a custom build configuration
ADD setEnvar.sh /tmp/wrf_hydro_nwm_public/trunk/NDHMS

# Build the code
RUN cd /tmp/wrf_hydro_nwm_public/trunk/NDHMS \
&& echo 2 | ./configure \
&& ./compile_offline_NoahMP.sh setEnvar.sh \
&& mkdir /home/docker/RUN  \
&& cp Run/* /home/docker/RUN \ 
&& mkdir /home/docker/RUN/FORCING \
&& mkdir /home/docker/RUN/DOMAIN \
&& mkdir /home/docker/RUN/OUTPUT 
#&& rm -rf /tmp/*

# add custom namelists
COPY --chown=docker:docker hydro.namelist /home/docker/RUN
COPY --chown=docker:docker namelist.hrldas /home/docker/RUN

# add code directory
WORKDIR /home/docker/RUN

ENTRYPOINT ["/bin/bash"]

