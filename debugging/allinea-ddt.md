# Allinea DDT

## Pre-Requisites

The Allinea Forge client can be downloaded for free:

## Remote Debugging Session

### Initial Configuration: Remote Launch Settings

The following configuration must be performed inside the Allinea Forge client:

* Click on the _RemoteLaunch_ drop down list
* Select _Configure..._
* Specify the following parameters:
  * Connection Name: An optional name for this connection
  * Host Name: The host name of the remote system you wish to connect to. Syntax:

    ```text
    [username]@hostname[:port]
    ```

    To login via one or more intermediate hosts \(e.g. a gateway\), enter the sequence of host names separated by spaces, e.g.:

    ```text
    gateway.mycompany.com frontend.mycompany.com
    ```

  * Remote Installation Directory: Product installation directory on the remote system Example:

    ```text
    /opt/allinea/forge
    ```

  * Remote Script: Commands to be run remotely at start Example:

    ```text
    module load forge
    ```

## _Reverse Connect_ Debugging Session

* Enable _Reverse Connect_ by modifying the job submission command:

  ```text
  ddt --connect mpirun <argument>
  ```

* Start Allinea Forge client
* Wait for client to connect to your remote system through SSH
* Launch job
* The Allinea Forge client will show the _Reverse Connect_ request

Note:

> The request will specify:
>
> * The host \(typically a batch compute node\) from where the request was made.
> * The command line summary.

