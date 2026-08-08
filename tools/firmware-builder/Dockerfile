FROM skylyrac/blocksds:slim-v1.22.1@sha256:55ca69d4f5685fb11af53bd4118485f118eee8d4cda6715dcd3c32006362ede4

RUN apt update && apt install -y \
    build-essential \
    cmake \
    gcc-arm-none-eabi \
    git \
    python3

ENV DLDITOOL=/opt/wonderful/thirdparty/blocksds/core/tools/dlditool/dlditool
ENV DOTNET_ROOT=/opt/dotnet

WORKDIR /opt

RUN mkdir -p dotnet && curl -s -L https://builds.dotnet.microsoft.com/dotnet/Sdk/9.0.308/dotnet-sdk-9.0.308-linux-arm64.tar.gz | tar xzf - -C dotnet

RUN git clone https://github.com/Gericom/DSRomEncryptor.git && cd /opt/DSRomEncryptor && git checkout f44682e79a5657dec62682ef80649506f561f00e
RUN git clone https://github.com/LNH-team/dspico-bootloader.git && cd /opt/dspico-bootloader && git checkout 29671d041fe2e497f8c39bae562e98d955afdbc5
RUN git clone https://github.com/LNH-team/dspico-dldi.git && cd /opt/dspico-dldi && git checkout 8ba45f65690bc40d9279e663e1d89ca806451cc1
RUN git clone https://github.com/LNH-team/dspico-firmware.git && cd /opt/dspico-firmware && git checkout 472c9d8e9957ad18df367f14b9cc337b9b887e65
RUN git clone https://github.com/LNH-team/dspico-wrfuxxed.git && cd /opt/dspico-wrfuxxed && git checkout 25e7d47f1854f367a902af6a15ed17edb262339f

RUN cd /opt/dspico-dldi && make
RUN cd /opt/dspico-bootloader && git submodule update --init && make
RUN $DLDITOOL /opt/dspico-dldi/DSpico.dldi /opt/dspico-bootloader/BOOTLOADER.nds

RUN cd /opt/DSRomEncryptor && /opt/dotnet/dotnet build
COPY private-inputs/biosnds7.rom private-inputs/biosdsi7.rom /opt/DSRomEncryptor/DSRomEncryptor/bin/Debug/net9.0/
RUN /opt/DSRomEncryptor/DSRomEncryptor/bin/Debug/net9.0/DSRomEncryptor /opt/dspico-bootloader/BOOTLOADER.nds /opt/dspico-firmware/roms/default.nds

RUN cd /opt/dspico-wrfuxxed && make
RUN $DLDITOOL /opt/dspico-dldi/DSpico.dldi /opt/dspico-wrfuxxed/uartBufv060.bin

RUN cd /opt/dspico-firmware && git submodule update --init
RUN cd /opt/dspico-firmware/pico-sdk && git submodule update --init

COPY private-inputs/wrfu.srl /opt/dspico-firmware/roms/dsimode.nds
RUN cp /opt/dspico-wrfuxxed/uartBufv060.bin /opt/dspico-firmware/data
RUN sed -i 's/^.*DSPICO_ENABLE_WRFUXXED.*$/  DSPICO_ENABLE_WRFUXXED/' /opt/dspico-firmware/CMakeLists.txt
RUN chmod +x /opt/dspico-firmware/compile.sh
RUN cd /opt/dspico-firmware && ./compile.sh

WORKDIR /build
