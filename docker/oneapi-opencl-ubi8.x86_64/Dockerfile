FROM registry.access.redhat.com/ubi8/ubi:8.5

################################################################################
# Initialize Intel oneAPI repository.
#
COPY ./oneAPI/oneAPI.repo /etc/yum.repos.d


################################################################################
# Install Intel oneAPI components by demand from repository (with cache cleanup).
# Example:
# RUN yum -y --disablerepo="*" --enablerepo="oneAPI" install intel-oneapi-runtime-libs.x86_64
#
RUN yum --disableplugin=subscription-manager -y update \
    && yum --disableplugin=subscription-manager -y --disablerepo="*" --enablerepo="oneAPI" install intel-oneapi-runtime-opencl.x86_64 \
    && yum -y install clinfo \
    && yum --disableplugin=subscription-manager clean all \
    && rm -rf /var/cache/yum


################################################################################
# Initialize LD_LIBRARY_PATH environmet variable.
# The result may looks like following: 
# LD_LIBRARY_PATH=/opt/intel/oneapi/compiler/2021.2.0/linux/compiler/lib/intel64_lin
# Verify inside of container : ldconfig -p | grep libsvml.so
#
RUN echo $(dirname $(find /opt/intel/oneapi -name libsvml.so)) >> /etc/ld.so.conf.d/oneapi.conf \
    && ldconfig


################################################################################
# OPTIONAL STEP: Initialize environmet variable for all profiles when required.
# Example:
# RUN echo "export TEST_ENV_VAR=$(dirname $(find /usr -name libc-*.so))" > /etc/profile.d/oneapivars.sh
#

