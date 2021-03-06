# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# COMPILE PROTOBUF STRUCTS REQUIRED TO BUILD MESOS.
###################################################
include(MesosProtobuf)

# Build the protobuf structs.
PROTOC_TO_INCLUDE_DIR(MESOS            mesos/mesos)
PROTOC_TO_INCLUDE_DIR(V1_MESOS         mesos/v1/mesos)
PROTOC_TO_INCLUDE_DIR(AUTHENTICATION   mesos/authentication/authentication)
PROTOC_TO_INCLUDE_DIR(AUTHORIZATION    mesos/authorizer/authorizer)
PROTOC_TO_INCLUDE_DIR(CONTAINERIZER    mesos/containerizer/containerizer)
PROTOC_TO_INCLUDE_DIR(DOCKER_V1        mesos/docker/v1)
PROTOC_TO_INCLUDE_DIR(DOCKER_V2        mesos/docker/v2)
PROTOC_TO_INCLUDE_DIR(EXECUTOR         mesos/executor/executor)
PROTOC_TO_INCLUDE_DIR(V1_EXECUTOR      mesos/v1/executor/executor)
PROTOC_TO_INCLUDE_DIR(FETCHER          mesos/fetcher/fetcher)
PROTOC_TO_INCLUDE_DIR(MAINTENANCE      mesos/maintenance/maintenance)
PROTOC_TO_INCLUDE_DIR(ALLOCATOR        mesos/master/allocator)
PROTOC_TO_INCLUDE_DIR(MODULE           mesos/module/module)
PROTOC_TO_INCLUDE_DIR(QUOTA            mesos/quota/quota)
PROTOC_TO_INCLUDE_DIR(SCHEDULER        mesos/scheduler/scheduler)
PROTOC_TO_INCLUDE_DIR(V1_SCHEDULER     mesos/v1/scheduler/scheduler)
PROTOC_TO_INCLUDE_DIR(ISOLATOR         mesos/slave/isolator)
PROTOC_TO_INCLUDE_DIR(OVERSUBSCRIPTION mesos/slave/oversubscription)
PROTOC_TO_INCLUDE_DIR(URI              mesos/uri/uri)

PROTOC_TO_SRC_DIR(REGISTRY master/registry)

PROTOC_TO_SRC_DIR(MESSAGES messages/messages)
PROTOC_TO_SRC_DIR(FLAGS    messages/flags)
PROTOC_TO_SRC_DIR(STATE    messages/state)

PROTOC_TO_SRC_DIR(MESSAGE slave/containerizer/mesos/provisioner/docker/message)

set(MESOS_PROTOBUF_SRC
  ${MESOS_PROTO_CC}
  ${V1_MESOS_PROTO_CC}
  ${AUTHENTICATION_PROTO_CC}
  ${AUTHORIZATION_PROTO_CC}
  ${CONTAINERIZER_PROTO_CC}
  ${DOCKER_V1_PROTO_CC}
  ${DOCKER_V2_PROTO_CC}
  ${EXECUTOR_PROTO_CC}
  ${V1_EXECUTOR_PROTO_CC}
  ${FETCHER_PROTO_CC}
  ${MAINTENANCE_PROTO_CC}
  ${ALLOCATOR_PROTO_CC}
  ${MODULE_PROTO_CC}
  ${QUOTA_PROTO_CC}
  ${SCHEDULER_PROTO_CC}
  ${V1_SCHEDULER_PROTO_CC}
  ${OVERSUBSCRIPTION_PROTO_CC}
  ${MESSAGES_PROTO_CC}
  ${FLAGS_PROTO_CC}
  ${STATE_PROTO_CC}
  ${ISOLATOR_PROTO_CC}
  ${REGISTRY_PROTO_CC}
  ${MESSAGE_PROTO_CC}
  ${URI_PROTO_CC}
  )

# Configure Mesos files.
########################
configure_file(
  ${MESOS_PUBLIC_INCLUDE_DIR}/mesos/version.hpp.in
  ${MESOS_BIN_INCLUDE_DIR}/mesos/version.hpp
  )

# SOURCE FILES FOR THE MESOS LIBRARY.
#####################################
set(AUTHENTICATION_SRC
  authentication/cram_md5/authenticatee.cpp
  authentication/cram_md5/authenticator.cpp
  authentication/cram_md5/auxprop.cpp
  )

set(AUTHORIZER_SRC
  authorizer/authorizer.cpp
  authorizer/local/authorizer.cpp
  )

set(COMMON_SRC
  common/attributes.cpp
  common/date_utils.cpp
  common/http.cpp
  common/protobuf_utils.cpp
  common/resources.cpp
  common/resources_utils.cpp
  common/type_utils.cpp
  common/values.cpp
  )

set(HDFS_SRC
  ${HDFS_SRC}
  hdfs/hdfs.cpp
  )

set(LINUX_SRC
  ${LINUX_SRC}
  linux/cgroups.cpp
  linux/fs.cpp
  linux/perf.cpp
  linux/systemd.cpp
  slave/containerizer/mesos/linux_launcher.cpp
  slave/containerizer/mesos/isolators/cgroups/cpushare.cpp
  slave/containerizer/mesos/isolators/cgroups/mem.cpp
  slave/containerizer/mesos/isolators/cgroups/perf_event.cpp
  slave/containerizer/mesos/isolators/filesystem/linux.cpp
  slave/containerizer/mesos/isolators/filesystem/posix.cpp
  slave/containerizer/mesos/isolators/filesystem/shared.cpp
  slave/containerizer/mesos/isolators/namespaces/pid.cpp
  slave/containerizer/mesos/provisioner/backends/bind.cpp
  )

set(DOCKER_SRC
  docker/docker.hpp
  docker/docker.cpp
  docker/executor.hpp
  docker/spec.cpp
  )

set(EXEC_SRC
  exec/exec.cpp
  )

set(FILES_SRC
  files/files.cpp
  )

set(HOOK_SRC
  hook/manager.cpp
  )

set(INTERNAL_SRC
  internal/devolve.cpp
  internal/evolve.cpp
  )

set(LOCAL_SRC
  local/local.cpp
  )

set(LOGGING_SRC
  logging/flags.cpp
  logging/logging.cpp
  )

set(MASTER_SRC
  master/contender.cpp
  master/constants.cpp
  master/detector.cpp
  master/flags.cpp
  master/http.cpp
  master/maintenance.cpp
  master/master.cpp
  master/metrics.cpp
  master/quota.cpp
  master/quota_handler.cpp
  master/registry.hpp
  master/registry.proto
  master/registrar.cpp
  master/repairer.cpp
  master/validation.cpp
  master/allocator/allocator.cpp
  master/allocator/mesos/hierarchical.cpp
  master/allocator/sorter/drf/sorter.cpp
  )

set(MODULE_SRC
  module/manager.cpp
  )

set(SCHED_SRC
  sched/constants.cpp
  sched/sched.cpp
  )

set(SCHEDULER_SRC
  scheduler/scheduler.cpp
  )

set(AGENT_SRC
  ${AGENT_SRC}
  slave/constants.cpp
  slave/state.cpp
  )

if (NOT WIN32)
  set(AGENT_SRC
    ${AGENT_SRC}
    slave/gc.cpp
    slave/flags.cpp
    slave/http.cpp
    slave/metrics.cpp
    slave/monitor.cpp
    slave/paths.cpp
    slave/qos_controller.cpp
    slave/qos_controllers/noop.cpp
    slave/resource_estimator.cpp
    slave/slave.cpp
    slave/status_update_manager.cpp
    slave/validation.cpp
    slave/containerizer/composing.cpp
    slave/containerizer/composing.hpp
    slave/containerizer/containerizer.cpp
    slave/containerizer/docker.cpp
    slave/containerizer/docker.hpp
    slave/containerizer/external_containerizer.cpp
    slave/containerizer/fetcher.cpp
    slave/containerizer/mesos/containerizer.cpp
    slave/containerizer/mesos/isolator.cpp
    slave/containerizer/mesos/launch.cpp
    slave/containerizer/mesos/launcher.cpp
    slave/containerizer/mesos/mount.cpp
    slave/containerizer/mesos/isolators/posix/disk.cpp
    slave/containerizer/mesos/provisioner/paths.cpp
    slave/containerizer/mesos/provisioner/provisioner.cpp
    slave/containerizer/mesos/provisioner/store.cpp
    slave/containerizer/mesos/provisioner/appc/paths.cpp
    slave/containerizer/mesos/provisioner/appc/spec.cpp
    slave/containerizer/mesos/provisioner/appc/store.cpp
    slave/containerizer/mesos/provisioner/backend.cpp
    slave/containerizer/mesos/provisioner/backends/copy.cpp
    slave/containerizer/mesos/provisioner/docker/message.proto
    slave/containerizer/mesos/provisioner/docker/metadata_manager.cpp
    slave/containerizer/mesos/provisioner/docker/paths.cpp
    slave/containerizer/mesos/provisioner/docker/puller.cpp
    slave/containerizer/mesos/provisioner/docker/registry_client.cpp
    slave/containerizer/mesos/provisioner/docker/registry_puller.cpp
    slave/containerizer/mesos/provisioner/docker/store.cpp
    slave/containerizer/mesos/provisioner/docker/token_manager.cpp
    slave/resource_estimators/noop.cpp
    )
endif (NOT WIN32)

set(URI_SRC
  uri/fetcher.cpp
  uri/utils.cpp
  uri/fetchers/curl.cpp
  uri/fetchers/hadoop.cpp
  )

set(USAGE_SRC
  usage/usage.cpp
  )

set(V1_SRC
  v1/attributes.cpp
  v1/mesos.cpp
  v1/resources.cpp
  v1/values.cpp
  )

set(VERSION_SRC
  version/version.cpp
  )

set(WATCHER_SRC
  watcher/whitelist_watcher.cpp
  )

set(ZOOKEEPER_SRC
  zookeeper/contender.cpp
  zookeeper/detector.cpp
  zookeeper/zookeeper.cpp
  zookeeper/authentication.cpp
  zookeeper/group.cpp
  )

set(MESSAGES_SRC
  ${MESSAGES_SRC}
  messages/messages.cpp
  )

set(MESOS_SRC
  ${MESOS_SRC}
  ${AGENT_SRC}
  ${MESOS_PROTOBUF_SRC}
  )

if (NOT WIN32)
  set(MESOS_SRC
    ${MESOS_SRC}
    ${AUTHENTICATION_SRC}
    ${AUTHORIZER_SRC}
    ${COMMON_SRC}
    ${DOCKER_SRC}
    ${EXEC_SRC}
    ${FILES_SRC}
    ${HDFS_SRC}
    ${HOOK_SRC}
    ${INTERNAL_SRC}
    ${LOCAL_SRC}
    ${LOGGING_SRC}
    ${MESSAGES_SRC}
    ${MASTER_SRC}
    ${MODULE_SRC}
    ${SCHED_SRC}
    ${SCHEDULER_SRC}
    ${URI_SRC}
    ${USAGE_SRC}
    ${V1_SRC}
    ${VERSION_SRC}
    ${WATCHER_SRC}
    ${ZOOKEEPER_SRC}
    )
endif (NOT WIN32)

# Include source for linux build.
if (LINUX)
  set(MESOS_SRC ${MESOS_SRC} ${LINUX_SRC})
endif (LINUX)

# INCLUDE DIRECTIVES FOR MESOS LIBRARY (generates, e.g., -I/path/to/thing
# on Linux).
#########################################################################
include_directories(${AGENT_INCLUDE_DIRS})

# LINKING LIBRARIES BY DIRECTORY (might generate, e.g., -L/path/to/thing on
# Linux).
###########################################################################
link_directories(${AGENT_LIB_DIRS})

# THE MESOS LIBRARY (generates, e.g., libmesos.so, etc., on Linux).
###################################################################
add_library(${MESOS_TARGET} ${MESOS_SRC})
set_target_properties(
  ${MESOS_TARGET} PROPERTIES
  VERSION ${MESOS_PACKAGE_VERSION}
  SOVERSION ${MESOS_PACKAGE_SOVERSION}
  )
add_dependencies(${MESOS_TARGET} ${AGENT_DEPENDENCIES})

# ADD LINKER FLAGS (generates, e.g., -lglog on Linux).
######################################################
target_link_libraries(${MESOS_TARGET} ${AGENT_LIBS})
