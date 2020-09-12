More about the search_targetpixelfile 

Signature: lk.search_targetpixelfile(target, radius=None, cadence='long', mission=('Kepler', 'K2', 'TESS'), quarter=None, month=None, campaign=None, sector=None, limit=None)
Docstring:
Searches the `public data archive at MAST <https://archive.stsci.edu>`_
for a Kepler or TESS `~lightkurve.targetpixelfile.TargetPixelFile`.

This function fetches a data table that lists the Target Pixel Files (TPFs)
that fall within a region of sky centered around the position of `target`
and within a cone of a given `radius`. If no value is provided for `radius`,
only a single target will be returned.

Parameters
----------
target : str, int, or `astropy.coordinates.SkyCoord` object
    Target around which to search. Valid inputs include:

        * The name of the object as a string, e.g. "Kepler-10".
        * The KIC or EPIC identifier as an integer, e.g. 11904151.
        * A coordinate string in decimal format, e.g. "285.67942179 +50.24130576".
        * A coordinate string in sexagesimal format, e.g. "19:02:43.1 +50:14:28.7".
        * An `astropy.coordinates.SkyCoord` object.
radius : float or `astropy.units.Quantity` object
    Conesearch radius.  If a float is given it will be assumed to be in
    units of arcseconds.  If `None` then we default to 0.0001 arcsec.
cadence : str
    'long' or 'short'.
mission : str, list of str
    'Kepler', 'K2', or 'TESS'. By default, all will be returned.
quarter, campaign, sector : int, list of ints
    Kepler Quarter, K2 Campaign, or TESS Sector number.
    By default all quarters/campaigns/sectors will be returned.
month : 1, 2, 3, 4 or list of int
    For Kepler's prime mission, there are three short-cadence
    TargetPixelFiles for each quarter, each covering one month.
    Hence, if cadence='short' you can specify month=1, 2, 3, or 4.
    By default all months will be returned.
limit : int
    Maximum number of products to return.

Returns
-------
result : :class:`SearchResult` object
    Object detailing the data products found.

Examples
--------
This example demonstrates how to use the `search_targetpixelfile()` function
to query and download data. Before instantiating a
`~lightkurve.targetpixelfile.KeplerTargetPixelFile` object or
downloading any science products, we can identify potential desired targets
with `search_targetpixelfile()`::

    >>> search_result = search_targetpixelfile('Kepler-10')  # doctest: +SKIP
    >>> print(search_result)  # doctest: +SKIP

The above code will query mast for Target Pixel Files (TPFs) available for
the known planet system Kepler-10, and display a table containing the
available science products. Because Kepler-10 was observed during 15 Quarters,
the table will have 15 entries. To obtain a
`~lightkurve.collections.TargetPixelFileCollection` object containing all
15 observations, use::

    >>> search_result.download_all()  # doctest: +SKIP

or we can download a single product by limiting our search::

    >>> tpf = search_targetpixelfile('Kepler-10', quarter=2).download()  # doctest: +SKIP

The above line of code will only download Quarter 2 and create a single
`~lightkurve.targetpixelfile.KeplerTargetPixelFile` object called `tpf`.

We can also pass a radius into `search_targetpixelfile` to perform a cone search::

    >>> search_targetpixelfile('Kepler-10', radius=100).targets  # doctest: +SKIP

This will display a table containing all targets within 100 arcseconds of Kepler-10.
We can download a `~lightkurve.collections.TargetPixelFileCollection` object
containing all available products for these targets in Quarter 4 with::

    >>> search_targetpixelfile('Kepler-10', radius=100, quarter=4).download_all()  # doctest: +SKIP
File:      /usr/local/lib/python3.6/dist-packages/lightkurve/search.py
Type:      function