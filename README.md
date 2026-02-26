Adapted from the original publication:
Norgeot, B., Muenzen, K., Peterson, T.A. et al. Protected Health Information filter (Philter): accurately and securely de-identifying free-text clinical notes. npj Digit. Med. 3, 57 (2020). https://doi.org/10.1038/s41746-020-0258-y

### Install
To install Philter-DirectStr, download the repository to the location of your project. Then install all requirements via

```bash
pip3 install -r requirements.txt
```

### Use

The code is modified from the original to allow for use directly on python strings, for example iterating through a list of strings or a dataframe, rather than requiring individual text files for each input note. Below is a minimal example

First, initialize philter, omitting input and output filepaths
```python
from philter_ucsf import philter
config = {
    "verbose": False,
    "run_eval": False,
    "outformat": "asterisk",
    "filters": "./philter_ucsf_master/philter_ucsf/configs/philter_delta.json",
}
nlp_philter = philter.Philter(config)
```

Then, you can run philter on any string, e.g.
```python
clinical_notes = [
    "Patient John Doe was seen on 01/01/2023.",
    "Dr. Smith prescribed 50mg of Aspirin to Jane."
]

deid_notes = []

for idx, note in enumerate(clinical_notes):
    safe_note = nlp_philter.process_string(note, doc_id=f"doc_{idx}")
    deid_notes.append(safe_note)
    
print(deid_notes)
```

The original functionality of philter is unchanged. Additional configuration options are:

**-i (input):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Path to the directory or the file that contains the clinical note(s), the default is `None`/<br/>
**-a (anno):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Path to the directory or the file that contains the PHI annotation(s), the default is `None`<br/>
**-o (output):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Path to the directory to save the PHI-reduced notes in, the default is `None`>
**-f (filters):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Path to the config file, the default is ./configs/philter_delta.json<br/>
**-x (xml):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Path to the json file that contains all xml data, the default is ./data/phi_notes.json<br/>
**-c (coords):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Output path to the json file that will contain the coordinate map data, the default is ./data/coordinates.json<br/>
**-v (verbose):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When verbose is true, will emit messages about script progress. The default is True<br/>
**-e (run_eval):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When run_eval is true, will run our eval script and emit summarized results to terminal<br/>
**-t (freq_table):**&nbsp;&nbsp;&nbsp;&nbsp;When freqtable is true, will output a unigram/bigram frequency table of all note words and their PHI/non-PHI counts. Default is False<br/>
**-n (initials):**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When initials is true, will include annotated initials PHI in recall/precision calculations. The default is True<br/>
**--eval_output:**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Path to the directory that the detailed eval files will be outputted to, the default is ./data/phi/<br/>
**--outputformat:**&nbsp;&nbsp;Define format of annotation, allowed values are \"asterisk\", \"i2b2\". Default is \"asterisk\"<br/>
**--ucsfformat:**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When ucsfformat is true, will adjust eval script for slightly different xml format. The default is False<br/>
**--prod:**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When prod is true, this will run the script with output in i2b2 xml format without running the eval script. The default is False<br/>
**--cachepos:**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Path to a directoy to store/load the pos data for all notes. If no path is specified then memory caching will be used<br/>/>
