# Internal documentation for PLUME members

## Github organization

To share codes or models, a [github organization](https://github.com/plumehub) has been set up for PLUME members.

To be added as a collaborator, please send your github login to aurelie.albert@uxxx.gxxxxxxx-axxxx.fr

Then, you can create your own project and collaborate on other's projects

## SUMMER storage

To share datasets, a 30Tb storage unit has been set up and is accessible via GRICAD for upload/download and computation and via opendap for download only (external sharing of data).

### GRICAD

If you do not have a GRICAD account yet, register [here](https://perseus.univ-grenoble-alpes.fr/create-account/portal) (select chercheur in contract type if you have a permanent position or are a post-doc, doctorant if you are PhD) and ask to join the pr-plume project.
 - Read carefully the [computation center documentation](https://gricad-doc.univ-grenoble-alpes.fr/en/)
 - Once logged to dahu (CPU) or bigfoot (GPU), you have access to the storage at : ```/summer/plume```

Check [here](gricad.md) for more informations on how to set up your access and compute on GRICAD

### Opendap

All the data that is put in ```/summer/plume``` is also visible directly at the adress : https://ige-meom-opendap.univ-grenoble-alpes.fr/thredds/catalog/meomopendap/extract/PLUME/catalog.html

You can download files by 
 - clicking on the name and then on the http server link
 - using some wget command :

```bash
wget https://ige-meom-opendap.univ-grenoble-alpes.fr/thredds/fileServer/meomopendap/extract/PLUME/README.txt
```

or something like : 

```bash
wget https://ige-meom-opendap.univ-grenoble-alpes.fr/thredds/fileServer/meomopendap/extract/SASIP/model-forcings/atmo_forcing/ERA5_Arctic/ERA5_v10_y{2011..2020}.nc
``` 

to retrieve multiple files


