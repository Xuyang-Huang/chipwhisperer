.. _install:

############
Installation
############

You get your choice of installing ChipWhisperer and its prerequisites: the easy way
or the hard way.

Basic
 * :ref:`install-windows-exe` (Windows 10 recommended)
 * :ref:`install-virtual-machine` (Recommended)

Advanced
 * :ref:`prerequisites`
     * :ref:`prerequisites-windows`
     * :ref:`prerequisites-linux`
     * :ref:`prerequisites-mac`

 * :ref:`Installation <install>`
     * :ref:`install-repo-git`
     * :ref:`install-repo-pypi`
     * :ref:`install-repo-releases`

 * :ref:`install-wm-ware`


.. _install-virtual-machine:

****************************
Virtual Machine (VirtualBox)
****************************

If this is your first time using the ChipWhisperer toolchain, the easiest
way to start is to use a virtual machine with everything already set up for
you. Note that Linux users may find it easier to do a manual install (
:ref:`prerequisites-linux`):

 * Install `VirtualBox`_. This program is freely available on Windows, Mac,
   and Linux.

 * Install the VirtualBox Extension pack, which can be found on the VirtualBox 
   downloads page linked above. This is necessary for the VM to interact with 
   the ChipWhisperer hardware.

 * Download a ChipWhisperer virtual machine image release or build it
   yourself using Vagrant.

 * Unzip the VirtualBox image, go to *Machine* > *Add* in VirtualBox and select
   the VM that was unzipped.

 * Verify that the VM boots.

.. note:: If you are on linux you need to add yourself to the *vboxusers*
    permission group using, so Virtual Box is given permission to access
    usb devices::

        sudo usermod -aG vboxusers <your username>

    Then refresh the groups by restarting your computer or logging out and in
    again.

Next, we'll need to update some passwords for the VM. Boot the virtual
machine then:

 * Log in (user: vagrant pass: vagrant).

 * Setup a new password for Jupyter. As of release 5.2.0, you will be prompted
   to set a password on startup if one doesn't exist for Jupyter. For older
   releases, simply type :code:`jupyter notebook password` in the command prompt,
   then set a password. Note
   that Jupyter will not start until this is done. This password will be
   needed to log into Jupyter, so make sure you record it as well.

 * Reboot the VM.

 * Once the VM is booted, you can connect to Jupyter via localhost:8888 (
   Firefox/Chrome ONLY). You will be asked for the password you set via
   jupyter notebook password

You shouldn't need to log in to the VM again to run Jupyter (which provides
the interface) as it should start automatically, but make sure you still
record the password you set for the vagrant account, as you will need to log
in to update ChipWhisperer.

You are now ready to use ChipWhisperer. Open Chrome/Firefox and
type **localhost:8888** into the address bar. This will give you access to
the Jupyter Notebook server running in the virtual machine.

.. _VirtualBox: https://www.virtualbox.org/wiki/Downloads

.. _install-windows-exe:

*****************
Windows Installer
*****************
.. note:: Beginning with ChipWhisperer 5.5, the Windows installer includes
          everything you need to run ChipWhisperer!

If you want to run a native Windows installation of ChipWhisperer, your best 
bet is to run the Windows installer, which takes care of getting the 
prerequisites for you. The steps for using the installer are as follows:

 * Navigate to the ChipWhisperer release page on Github: `releases`_

 * Find the latest ChipWhisperer Windows install executable (currently 
   :code:`Chipwhisperer.v5.5.0.Setup.64-bit.exe`)
 
 * Run the executable and choose the path you want to install ChipWhisperer at. 
   You must have read/write permissions for the location you install to, so 
   avoid installing in a location like :code:`C:\Program Files` or the like. The 
   default install location (the user's home directory) will work for most users.

 * Choose whether or not you want to create a desktop shortcut for running 
   ChipWhisperer and whether or not you want to install make and compilers (we recommend that you
   do).

If you're on firmware x.23 or newer, you're all set! Drivers will be automatically installed when you plug your ChipWhisperer in.
Otherwise, you may need to upgrade your firmware before continuing. See our section :ref:`driverless_windows` for more information.

With this, you now have a fully functioning ChipWhisperer install. Run the 
ChipWhisperer app, then navigate to the Jupyter folder, where tutorials for 
running ChipWhisperer are located.

.. _releases: https://github.com/newaetech/chipwhisperer/releases

.. _install-repo:

*************
ChipWhisperer
*************

.. note:: You must have the :ref:`prerequisites` for your system installed
	before continuing with the installation of the repository.

.. note:: You may have to replace all the calls to **python** on the command line with
    whatever gives you access to the python version you installed. On GNU/Linux you will
    probably use **python3**, or you can use the full path to the python interpreter.
    It is not required but recommended to use a virtual environment.

After satisfying prerequisites for your system, install the ChipWhisperer
repository/package using one of:

:ref:`install-repo-git` (Recommended)
  The best way to get the ChipWhisperer software, as well as
  drivers, hardware source code, and Jupyter Notebook tutorials.


:ref:`install-repo-pypi`
	The classic :code:`pip install chipwhisperer`. Does not install
	the hardware source code or Jupyter Notebooks.

:ref:`install-repo-releases`
	Get the latest stable release from the GitHub repository. The release includes
	hardware source code, but no Jupyter Notebooks currently.

.. _install-repo-git:

Git
===

The recommended way to install ChipWhisperer natively is by cloning it from 
Git. By default this will pull in the develop version, which has all the 
latest features/bug fixes, but we also keep each major release on master.

.. note::

   On Unix based OS (Mac, Linux, etc), python often links to python2. You
   may need to replace python and pip calls with python3 and pip3 calls,
   respectively

If you have Git already set up, this is easy to do:

.. code:: bash

    git clone https://github.com/newaetech/chipwhisperer.git
    cd chipwhisperer

    # To get the jupyter notebook tutorials
    git submodule update --init jupyter
    python -m pip install -r jupyter/requirements.txt --user

    # enable jupyter interactive widgets
    jupyter nbextension enable --py widgetsnbextension

    # use pip to install in develop mode
    python -m pip install -e . --user

The user flag installs ChipWhisperer in the user's local python
site-packages directory.

You may also want the OpenADC software, which is necessary to build new
firmware for the ChipWhisperer FPGA. This is unnecessary for most users. If
you need it:

.. code::

    cd ..
    git submodule update --init openadc
    cd openadc/controlsw/python
    python -m pip install -e . --user

Once ChipWhisperer is installed, you can :ref:`run chipwhisperer <starting>`.

.. _install-repo-pypi:

PyPi
====

If you want to use **chipwhisperer** as a standalone python package and are not
interested in having all the tutorials and extra jupyter notebook stuff, this
installation method is for you::

    pip install chipwhisperer

Will install the *chipwhisperer/software/chipwhisperer* python package in your
site packages. Now you can go play around with the :ref:`Python API <api>`, or
take a look at some example :ref:`tutorials <tutorials>` The tutorials are all
written in jupyter notebook, which you don't have using this installation
method. However, you can still take a look at the procedure and the code, and
use it as an example of what can be accomplished using **chipwhisperer**.

.. _install-repo-releases:

GitHub Releases
===============

ChipWhisperer is also available as a Github release. This version won't come with
Jupyter tutorials and will be more difficult to update, so it isn't recommended.

First, download a ChipWhisperer release. You can get these from the `releases`_ page.
Generally, the latest release is a good choice, but you might need an older version
for various reasons. You want the source code in .zip or .tar.gz format - not a VBox
image.

Next, uncompress your downloaded source code somewhere. Generally, 'somewhere' will
become your ChipWhisperer working directory. For example, on Windows, you might
want to use :code:`C:\\chipwhisperer\\`.

Once you've got the file, install the python dependencies and run the Python
install procedure (setup.py) using pip. Use the -e flag for develop mode to indicate
that the files will probably be changing frequently. To do this, open a terminal and run
the following, adjusting paths as needed:

.. code:: bash

    cd chipwhisperer
    python -m pip install -e . --user

    # to be able to run jupyter and the tutorials
    pip install -r jupyter/requirements.txt --user

    # enable jpyter interactive widgets
    jupyter nbextension enable --py widgetsnbextension


To test, run python and try importing the **chipwhisperer** module:

.. code:: python

    >>> import chipwhisperer as cw

If you want to run the tutorials you can now start the
:ref:`Jupyter Notebook server <starting>`.


.. _releases: https://github.com/newaetech/chipwhisperer/releases


.. _install-wm-ware:

*************************
Virtual Machine (VMWare)
*************************

For various reasons, such as licensing and USB support, users may prefer to run 
ChipWhisperer through VMWare instead of VirtualBox. A VMWare compatable image is not
provided with ChipWhisperer releases, but such an image can be easily converted
from the provided image using VirtualBox

 * Install `VirtualBox`_

 * Download a ChipWhisperer virtual machine image release or build it
   yourself using Vagrant. Virtual machine images can be found on our Github
   `releases`_

 * Add the VM image to VirtualBox

 * Right click on the image in VirtualBox and select :code:`Export to OCI` 

 * Select :code:`OVF Format 1.0` and export using the default settings.

 * The resulting :code:`.ovf` file can be opened in VMWare. VMWare may complain
   about the file not following OVF specifications. If this happens, hit 
   :code:`retry`.

You should now have a working VMWare image. Boot the VM and add passwords as described in :ref:`install-virtual-machine`
Before logging out, run the following command and record the :code:`eth0` IP Address:

.. code:: bash

    ip addr

The final step is to setup VMWare port forwarding:

 * If you have VMWare Player, you'll need to install VMWare Workstation Pro.
   The required utility tool does not require a license to run, so Workstation
   Pro can be installed without purchasing the software. If you're already
   running Workstation Pro, you can skip this step.

 * Navigate to the folder where VMWare Workstation Pro is installed and run 
   :code:`vmnetcfg.exe`

 * Click the :code:`Change Settings` button.

 * Click on the :code:`NAT` table entry (typically VMnet8) and click on :code:`NAT Settings...` 
   Take note of the Subnet Address of this entry

 * Under the Port Fowarding table, click :code:`Add` and fill in the following settings:
     * :code:`Host port:                  8888`
     * :code:`Type:                       TCP`
     * :code:`Virtual machine IP address: <subnet address>`
     * :code:`Virtual machine port:       8888`
     * :code:`Description:                Jupyter` (optional)

 * Hit :code:`OK` until :code:`vmnetcfg.exe` is closed

You should now be able to open the VM and connect to :code:`<eth0 IP>:8888`, replacing
:code:`<eth0 IP>` with the IP address you recorded after running :code:`ip addr`. 


 

