# Start from the existing image
FROM judge0/compilers:1.4.0

# Install dependencies required for building GCC
RUN apt-get update && \
  apt-get install -y build-essential libgmp-dev libmpfr-dev libmpc-dev && \
  rm -rf /var/lib/apt/lists/*

# Download and install python 3.12.3
ENV PYTHON_VERSION 3.12.3
RUN curl -fSsL "https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tar.xz" -o /tmp/python-$PYTHON_VERSION.tar.xz && \
  mkdir /tmp/python-$PYTHON_VERSION && \
  tar -xf /tmp/python-$PYTHON_VERSION.tar.xz -C /tmp/python-$PYTHON_VERSION --strip-components=1 && \
  rm /tmp/python-$PYTHON_VERSION.tar.xz && \
  cd /tmp/python-$PYTHON_VERSION && \
  ./configure --prefix=/usr/local/python-$PYTHON_VERSION && \
  make -j$(nproc) && \
  make -j$(nproc) install && \
  rm -rf /tmp/python-$PYTHON_VERSION

# Download and install GCC 11.3.0
ENV GCC_VERSION 11.3.0
RUN curl -fSsL "https://ftpmirror.gnu.org/gcc/gcc-$GCC_VERSION/gcc-$GCC_VERSION.tar.gz" -o /tmp/gcc-$GCC_VERSION.tar.gz && \
  mkdir /tmp/gcc-$GCC_VERSION && \
  tar -xf /tmp/gcc-$GCC_VERSION.tar.gz -C /tmp/gcc-$GCC_VERSION --strip-components=1 && \
  rm /tmp/gcc-$GCC_VERSION.tar.gz && \
  cd /tmp/gcc-$GCC_VERSION && \
  ./contrib/download_prerequisites && \
  { rm *.tar.* || true; } && \
  tmpdir="$(mktemp -d)" && \
  cd "$tmpdir"; \
  /tmp/gcc-$GCC_VERSION/configure --disable-multilib --enable-languages=c,c++ --prefix=/usr/local/gcc-$GCC_VERSION && \
  make -j$(nproc) && \
  make -j$(nproc) install-strip && \
  rm -rf /tmp/*; 

# Add the new GCC binary to the PATH
ENV PATH /usr/local/gcc-$GCC_VERSION/bin:$PATH

# Add any additional instructions here
LABEL version="1.4.0-gcc11.3.0-py3.12.3"
