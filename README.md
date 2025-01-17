# ndx-external-resources Extension for NWB

## Installation


## Usage

```python
# TODO move these functions to PyNWB core

import datetime
from pynwb import TimeSeries, NWBHDF5IO
from pynwb.core import DynamicTable

from ndx_external_resources import ERNWBFile


nwbfile = ERNWBFile(
    session_description='session_description',
    identifier='identifier',
    session_start_time=datetime.datetime.now(datetime.timezone.utc)
)

container = TimeSeries(
    name='test_ts',
    data=[1, 2, 3],
    unit='meters',
    timestamps=[0.1, 0.2, 0.3],
)
nwbfile.add_acquisition(container)

table = DynamicTable(name='test_table', description='test table description')
table.add_column(name='test_col', description='test column description')
table.add_row(test_col='Mouse')

nwbfile.add_acquisition(table)

nwbfile.external_resources.add_ref(
    container=container,
    field='unit',
    key='meters',
    resource_name='SI_Ontology',
    resource_uri='',
    entity_id='5',
    entity_uri='',
)

nwbfile.external_resources.add_ref(
    container=table,
    field='test_col',
    key='Mouse',
    resource_name='NCBI_Taxonomy',
    resource_uri='https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi',
    entity_id='10090',
    entity_uri='https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?mode=info&id=10090',
)

path = 'test.nwb'
with NWBHDF5IO(path, mode='w') as io:
    io.write(nwbfile)

with NWBHDF5IO(path, mode='r', load_namespaces=True) as io:
    read_nwbfile = io.read()
    read_container = nwbfile.acquisition['test_ts']
    read_table = nwbfile.acquisition['test_table']
    print(nwbfile.external_resources.get_object_resources(read_container, 'unit'))
    # TODO expand this example after paths for objects/keys is improved
```

This extension was created using [ndx-template](https://github.com/nwb-extensions/ndx-template).
