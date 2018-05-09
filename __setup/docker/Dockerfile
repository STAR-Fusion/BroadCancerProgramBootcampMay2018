FROM trinityctat/ctatfusion:1.4.0

MAINTAINER Brian Haas (bhaas@broadinstitute.org)

RUN apt-get update && apt-get install -y build-essential curl unzip apache2 supervisor \
     default-jre wget git ftp nano python2.7-dev python-pip openssl openssh-server \
          vim emacs perl gcc g++ perl python automake make \
                  wget git curl libdb-dev \
                      zlib1g-dev bzip2 libncurses5-dev \
                          texlive-latex-base \
                              default-jre dialog screen \
                                  python-pip python-dev \
                                      && apt-get clean


RUN groupadd -g 2000 training && useradd -m -u 2000 -g 2000 training
RUN echo 'training:training' | chpasswd
RUN chsh -s /bin/bash training
ENV HOME=/home/training
RUN echo "alias ll='ls -la -G'" >> /home/training/.profile
RUN usermod -G training,www-data training


WORKDIR /usr/local/src


#########
### GateOne SSH interface
#########
RUN git clone https://github.com/liftoff/GateOne/ && \
    cd GateOne && python setup.py install && \
    pip install tornado==4.5.3 && \
    python run_gateone.py --configure 



# ports
EXPOSE 22 80 443 8787


# init

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord","-c","/etc/supervisor/conf.d/supervisord.conf"]

ENV TERM=xterm-256color

COPY 000-default.conf /etc/apache2/sites-available/

WORKDIR $HOME

ENV STAR_FUSION_HOME /usr/local/src/STAR-Fusion

RUN apt-get install -y bsdmainutils