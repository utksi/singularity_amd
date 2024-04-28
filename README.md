# singularity_amd

1. torch_pyg_e3nn_mace_rocm5.7.def
   
   - docker: rocm/dev-ubuntu-22.04:5.7-complete via python:3.10.12
     
   - pytorch compatible with AMD's rocm v5.7.0 (not 5.7.1):
     
     torch==2.2.2 torchvision==0.17.2 torchaudio==2.2.2 --index-url https://download.pytorch.org/whl/rom5.7
     https://pytorch.org/get-started/previous-versions/sudo
     
   - pytorch_geometric (and optional dependencies) compatible with ROCM:
     
     https://github.com/Looong01/pyg-rocm-build.git
     
     In case it doesn't exist at some point, the versions are:
     pytorch_geometric-2.5.2, pytorch_scatter-2.1.2, pytorch_sparse-0.6.18, pytorch_cluster-1.6.3, pytorch_spline_conv-1.2.2


   - MACE (https://github.com/ACEsuit/mace):
     
     For now (v0.3.4), it doesnt want to upgrade any dependencies of torch/pyg
     e3nn v0.4.4, scipy v1.13.0, numpy v1.26.3

   - numba:
     
     current version v0.59.1, dep. is llvmlite 0.42.0

   - BUILD (on your local linux machine/VM)
     
     1. Create a temporary buid_dir and mount it:
        
        sudo mkdir /buildtmp
        sudo chmod 1777 /buildtmp
        export SINGULARITY_TMPDIR=/buildtmp
        
     3. Build the container (in sandbox mode, alternatively replace --sandbox by <name.sif> for an immutable .sif container):
        
        sudo SINGULARITY_TMPDIR=/buildtmp singularity build --sandbox torch_pyg_e3nn_mace_rocm5.7.def
     
     5. Check for usage, if none, unmount build_dir, check if it is successfully unmounted and (optionally) delete it:
        
        sudo lsof +D /buildtmp
        sudo umount /buildtmp
        mount | grep buildtmp
        sudo rm -r /buildtmp

   - Making changes to the container:
     
     1. interactively:
        
        sudo singularity shell --writable torch_pyg_e3nn_mace_rocm5.7
        <Make changes>
        exit
        
     3. Non-interactively (create a work dir to be bound later to hpc filesystem, for example):
        
        sudo singularity exec --writable torch_pyg_e3nn_mace_rocm5.7 mkdir work
        
