# Casper NCTL - Docker Container for Kairos

Customized NCTL that incorporates increased Casper node limits, to allow running [Kairos L2](https://github.com/cspr-rad/kairos):

```diff
diff --git a/casper-nctl.Dockerfile b/casper-nctl.Dockerfile
index 473ff59..5814607 100644
--- a/casper-nctl.Dockerfile
+++ b/casper-nctl.Dockerfile
@@ -32,6 +32,10 @@ RUN git clone https://github.com/casper-network/casper-node-launcher.git ~/caspe
 RUN git clone -b $CLIENT_GITBRANCH https://github.com/casper-ecosystem/casper-client-rs ~/casper-client-rs \
     && cd ~/casper-client-rs && cargo build --release
 RUN git clone -b $NODE_GITBRANCH https://github.com/casper-network/casper-node.git ~/casper-node \
+    && sed -i 's|\(max_deploy_size = \)[0-9_]\+|\14_194_304|' ~/casper-node/resources/local/chainspec.toml.in \
+    && sed -i 's|\(max_body_bytes = \)[0-9_]\+|\18_388_608|g' ~/casper-node/resources/local/config.toml \
+    && sed -i 's|\(session_args_max_length = \)[0-9_]\+|\110_000_000|' ~/casper-node/resources/local/chainspec.toml.in \
+    && sed -i 's|\(max_memory = \)[0-9_]\+|\1640|' ~/casper-node/resources/local/chainspec.toml.in \
     && source ~/casper-node/utils/nctl/sh/assets/compile.sh

 # run clean-build-artifacts.sh to remove intermediate files and keep the image lighter
```

Published in Docker Hub as [koxu1996/casper-nctl:v156-kairos](https://hub.docker.com/layers/koxu1996/casper-nctl/v156-kairos/images/sha256-3b860f31b5de4c01b6346f068c8cb7356b49653973994ccc1d8792916a769a0f?context=explore), built with:

```sh
$ docker build -f casper-nctl.Dockerfile --build-arg NODE_GITBRANCH=release-1.5.6 --build-arg CLIENT_GITBRANCH=release-2.0.0 -t koxu1996/casper-nctl:v156-kairos .
```
