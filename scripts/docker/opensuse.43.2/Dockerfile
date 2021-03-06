#
# Copyright (c) .NET Foundation and contributors. All rights reserved.
# Licensed under the MIT license. See LICENSE file in the project root for full license information.
#

FROM microsoft/dotnet-buildtools-prereqs:opensuse-42.3-d46ee12-20180327014902

# Install the base toolchain we need to build anything (clang, cmake, make and the like)
# this does not include libraries that we need to compile different projects, we'd like
# them in a different layer.
RUN zypper -n install binutils \
              cmake \
              which \
              gcc \
              llvm-clang \
              tar \
              ncurses-utils \
              curl \
              git \
              sudo && \
    zypper clean -a

# Dependencies of CoreCLR and CoreFX.

RUN zypper -n install --force-resolution \
                      libunwind \
                      libicu \
                      lttng-ust \
                      libuuid1 \
                      libopenssl1_0_0 \
                      libcurl4 \
                      krb5 && \
    zypper clean -a

# Setup User to match Host User, and give superuser permissions
ARG USER_ID=0
RUN useradd -m code_executor -u ${USER_ID} -g wheel
RUN echo 'code_executor ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# With the User Change, we need to change permissions on these directories
RUN chmod -R a+rwx /usr/local
RUN chmod -R a+rwx /home
RUN chmod -R 755 /usr/lib/sudo

# Set user to the one we just created
USER ${USER_ID}

# Set working directory
WORKDIR /opt/code