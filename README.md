# Automated FDSN DOI Look-up Tool

This script automates the process of generating a list of **DOIs for seismic data networks**, using metadata from the **FDSN (International Federation of Digital Seismograph Networks)** registry.

## 📋 Features

- Extracts unique FDSN network codes and associated years from locally stored seismic data
- Queries the FDSN registry for network metadata
- Outputs a clean, de-duplicated text file containing:
  - Network code
  - Network name
  - Start and end years
  - DOI (if available)
 
## Usage
You will need:
 - Python 3
 - Internet access

**Inputs:**  
You will need a list of unique network codes and years for the seismic data:  
`network_codes = [('IU', '2000'),('US', '2005')]`

### Making a network_codes list:
#### 1. Generate a network_codes list

Use the `make_network_codes_list()` function to generate a list of unique network/year combinations from your locally stored seismic data.
For this option you need to have `make_list=True`.  

```python
make_list = True
if make_list:
    parent_data_dir = "Processed/"   # change to your parent_data_dir
    output_file = "network_years.txt"
    network_codes = make_network_codes_list(parent_data_dir, output_file)
```

**and** 

Your seismic data should be organized as follows:

```python
parent_data_dir/  
└── YYYYMMDDHHMMSS/         # One folder per event (e.g. 20100523224651)  
    └── Data/  
        ├── IU.ANMO.BHZ.MSEED  
        ├── US.LBNH.BHE.MSEED  
        └── ...  
```
Each `Data/` folder must contain miniSEED files name formatted as NETWORK.STATION.CHANNEL.MSEED   
The network_codes list will be made for all event directories in the parent_data_dir.

#### 2. Manually Define/Load/Code your own a Network Code List 
There is an allocated space in the script to write your own code to generate (or load in) a networks_codes list from your data storage setup.
```python
make_list = False
if make_list:
    ...
else:
    '''modify to load/make your own network_codes list'''
    pass
```

### Run
```bash
python FDSN_DOIs.py
```
This will generate automatically a text file with network code, name, start and end years, and DOI (if available), from the FDSN website using `create_dois_list(network_codes)` function. Duplicate networks/DOIs will not be written out into the text file.  

The output file is named dois_list.txt, it contains one line per unique network. Example line below:   
  
| Network Code | Network Name                                                                                                                  | Start Year | End Year | DOI                                                |
|--------------|--------------------------------------------------------------------------------------------------------------------------------|------------|----------|-----------------------------------------------------|
| 1M           | Irish Seismological Lithospheric Experiment / Irish Seismological Upper Mantle Experiment (ISLE/ISUME)                        | 2003       | 2015     | [https://doi.org/10.14470/6Z7558576550](https://doi.org/10.14470/6Z7558576550) |

