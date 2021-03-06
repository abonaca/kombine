kombine
=======

``kombine`` is an ensemble sampler built for efficiently exploring multimodal distributions.
By using estimates of ensemble's instantaneous distribution as a proposal, it achieves very
fast burnin, followed by sampling with very short autocorrelation times.
Contents:


Example Usage
-------------

Construct an 8-D bimodal target distribution::

    import numpy as np

    class Target(object):
        def __init__(self, ndim, nmodes):
            self.ivar = 1. / np.random.rand(ndim)
            std = np.sqrt(1/self.ivar)
            self.means = 5 * std * np.arange(nmodes)

        def __call__(self, x):
            ivar, means = self.ivar, self.means
            lnprobs = [-np.sum(ivar * (x - mu) ** 2)/2 for mu in means]
            return np.logaddexp.reduce(lnprobs)

    ndim, nmode = 8, 2

Sample the target distribution::

    import kombine

    nwalkers = 500
    sampler = kombine.Sampler(nwalkers, ndim, lnprob)

    p0 = 5 * (5 * np.random.rand(nwalkers, ndim) - 1)
    sampler.burnin(p0)

User Guide
----------

.. toctree::
    :maxdepth: 2

    installation
    tutorial

API Documentation
-----------------

 * :ref:`genindex`
 * :ref:`modindex`
 * :ref:`search`

Contributors
------------

.. include:: ../AUTHORS.rst


