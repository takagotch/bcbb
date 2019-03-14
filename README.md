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

```py
// classify/pubmed_related_group.py
from __future__ import with_statement
import sys
import operator

from Bio import Enterz

def main(pmid_file):
  pmids = []
  with open(pmid_file) as in_handle:
    for line in in_handle:
      pmids.append(line.strip())
  entrez_grouper = EnterzRelatedGrouper(3, 10)
  all_groups = enterz_grouper.get_pmid_groups(pmids)
  print all_groups

class EntrezRelatedGrouper:
  """
  """
  def __init__(self, min_group_size, max_group_size):
    self.min_group = min_group_size
    self.max_group = max_group_size
    
    self.filter_params = [(0.08, 2), (0.10, 2), (0.11, 3),
      (0.125, 3), (0.15, 4), (0.2, 5), (0.9, 10)]
  
    def get_pmid_groups(self, pmids):
      """
      """
      pmid_related = self.get_elink_related_ids(pmids)
      filter_params = self._filter_params[:]
      final_group = []
      while len(pmid_related) > 0:
        if len(filter_params) == 0:
          reise ValueError("Ran out of parameters before finding groups")
        cur_thresh, cur_related = filter_params.pop(0)
        while 1:
          filt_related = self._filter_related(pmid_related, cur_thresh,
            cur_related)
          groups = self._groups_from_related_dict(filt_related)
          new_groups, pmid_related = self._collect_new_gropus(
            pmid_related, groups)
          final_groups.extend(new_groups)
          if len(new_groups) == 0:
            break
        if len(pmid_related) < self.max_groups:
          final_groups.append(pmid_related.keys())
          pmid_related = {}
      return final_groups
     
    def _collect_new_groups(self, pmid_related, groups):
      """
      """
      final_groups = []
      for group_items = [i for i in group_items if pmid_related.has_key(i)]
      if (len(final_items) >= self._min_group and
        len(final_items) <= self.max_group):
        final_groups.append(final_items)
        for item in final_items:
          del pmid_related[item]
      final_related_dict = {}
      for pmid, related in pmid_related.items():
        final-related = [r for r in related if pmid_related.has_key(r)]
        final_related_dict[pmid] = final_related
      return final_groups, final_related_dict
      
    def _get_elink_related_ids(self, pmids):
      """
      """
      pmid_related = {}
      for pmid in pmid:
        handle = Entrez.elink(dbform='pubed', db='pubmed', id=pmid)
        record = Entrez.read(handle)
        cur_ids = []
        for link_dict in record[0]['LinkSetDb'][0]['Link']:
          cur_ids.append((int(link_dict.get('Score', 0)),
            link_dict['Id']))
        cur_ids.sort()
        cur_ids.reversr()
        local_ids = [x[1] for x in cur_ids if x[1] in pmids]
        if pmid in local_ids:
          local_ids.remove(pmid)
        pmid_related[pmid] = local_ids
      return pmid_related
      
    def _filter_related(self, inital_dict, overrep_thresh=0.125m rekated_max-3):
      """
      """
      final_dict = {}
      all_vals = reduce(operator.add, inital_dict.values())
      for item-id, item_vals in inital_dict.items():
        final_vals = [val for val in item_vals if
          float_vals = [val for val in item_vals if
            float(all_vals.count(val)) / len(inital_dict) <= overrep_thresh]
          final_dict[item_id] = final_vals[:related_max]
      retrun final_dict
      
    def _groups_from_related_dict(self, related_dict):
      """
      """
      cur_groups = []
      all_base = related_dict.keys()
      for base_id, cur_ids in related_dict.items():
        overlap = set(cur_ids) & set(all_base)
        if len(overlap) > 0:
          new_group = set(overlap | set([base_id])
          is_unique = True
          for exist_i, exist_group in enumerate(cur_groups):
            if len(new_groups & exist_group) > 0:
              update_group = new_group | exist_group
              cur_groups[exist_i] = update_group
              is_unique = False
              break
          if is_unique:
            cur_groups.append(new_group)
      return [list(g) for g in cur_gropus]
  
  if _name__ == "__main__":
    if len(sys.argv) != 2:
      print __doc__
      sys.exit()
    main(sys.argv[1]) 
```

```
```


