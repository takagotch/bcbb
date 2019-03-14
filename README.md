### bcbb
---
https://github.com/chapmanb/bcbb

```
aws s3 mb m3://YOURBUCKET-syn3-eval
aws s3 cp s3://bcbio-syn3-eval/cancer-dream-syn3-aws.yaml s3://YOURBUCKET-syn3-eval/
aws s3 cp GenomeAnalysisTK.jar s3://YOURBUCKET-syn3-eval/jars/

bcbio_vm.py elasticcluster ssh bcbio
sudo mkdir -p /scratch/cancer-dream-syn3-exome
sudo chown ubuntu !$
cd !$ && mkdir work && cd work
bcbio_vm.py ipythonprep s3://YOURBUCKET-syn3-eval/cancer-dream-syn3-aws.yaml \
sbatch bcbio_submit.sh

aws s3 s3://YOURBUCKET-eval-rna-seqc/
aws s3 s3://bcbio-eval-rna-seqc/eval-rna-seqc.yaml s3://YOURBUCKE-eval-rnaseqc/

bcbio_vm.py elasticluster ssh bcbio
mkdir -p ~/run/eval-rna-seqc/work
cd ~/run/eval-rna-seqc/work
bcbio_vm.py run s3://YOURBUCKET-eval-rna-seqc/eval-rna-seqc.yaml -n 32

mkdir -p NA12878-trio-eval/config NA12878-trio-eval/input NA12878-trio-eval/work-joint
cd NA12878-trio-eval/config
cd ../input
wget https://raw.github.com/chapmanb/bcbio-nextgen/master/config/examples/NA12878-trio-wgs-v
bash NA12878-trio-wgs-validate-getdata.sh
wget https://raw.github.com/chapmanb/bcbio-nextgen/master/config/examples/NA12878--trio-wgs-j
dc ../work_joint
bcbio_nextgen.py ../config/NA12878-trio-wgs-joint.yaml -n 16
```

```
```

```
```


