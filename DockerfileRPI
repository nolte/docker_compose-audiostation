FROM sdhibit/rpi-raspbian


RUN useradd -ms /bin/bash snapcast
RUN usermod -a -G audio snapcast

RUN mkdir -p /tmp/sharesound ; chown -R snapcast:audio /tmp/sharesound ; chmod 0777 /tmp/sharesound
VOLUME ["/tmp/sharesound"]

USER snapcast

RUN mkfifo /tmp/sharesound/snapfifo

CMD ["true"]
