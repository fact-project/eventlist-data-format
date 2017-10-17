# eventlist-data-format

Specification of our high level hdf5 data format.

This specification deals with the properties of events, that
can be represented by single numbers per event.

The event list is therefore a collections of 2d tables necessary
to perform gamma-ray analysis.



## Data Format

We use `h5py` to write `hdf5` files containing different `group`s 
and in those `group`s, one `dataset` per attribute.

The names of `group`s and `dataset`s shall be `snake_case`.

## Measured events

HDF5 files for measured data shall contain the following groups:

### `events`

Event wise data.

A measured event is uniquely identified by
the three columns 

* `night`
* `run_id`
* `event_num`

More columns can be added, e.g. for FACT-Tools output or
by using the `klaas_apply_<...>` scripts.

There are some conventions for the column names:

* `theta` shall be in mm
* `theta_deg` shall be in degrees
* Features for the off positions shall be called `<name>_off_<N>`
e.g. `theta_deg_off_2`


### `runs`

Metadata for the runs.

Runs are uniquely identified by the two columns

* `night`
* `run_id`

For gamma data analysis, the `runs` group should also contain the following columns: 

* `source`: The name of the source as in our `factdata.Source` mysql table
* `ontime`: The ontime of the run in seconds as defined by (`run_stop` - `run_start`) * `effective_ontime`
* `right_ascension`: right ascension in hour angle of the run
* `declination`: declination in degree of the run
* `zenith`: mean zenith in degree for the run
* `azimuth`: mean azimuth in degree for the run
* `run_start`: ISO timestamp of the beginning of the run
* `run_stop`: ISO timestamp of the end of the run


## Simulated events

HDF5 files for measured data shall contain the following groups:


### `events`

Event wise data of analsis results, e.g. after FACT-Tools.
These will usually be much less events than in the `corsika_events` 
or `ceres_events` group because of the trigger and applied analysis cuts.

A simulated event is uniquely identified by the three columns 

* `run_id`: The CORSIKA run id
* `event_num`: The CORSIKA event id
* `n_reuse`: The CORSIKA event id

**FACT-Tools does not store `run_id` and `n_reuse` at the moment, so
the information is lost and a direct mapping of events in different tables is thus not possible, see https://github.com/fact-project/fact-tools/issues/248**

### `corsika_events`

All eventwise simulation truth values that were used to produce the
events in the `events` group. 

### `corsika_runs`

All runwise simulation truth values  / settings for the runs that were used to produce the events in the `events` group. 

### `ceres_events`

Event wise data for all events after ceres simulation, e.g. all events that triggered the telescope.
