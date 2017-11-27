# Allinea DDT

## *Reverse Connect* Debugging Session

* Enable *Reverse Connect* by modifying the job submission command:
```
ddt --connect mpirun <argument>
```
* Start Allinea Forge client

* Wait for client to connect to your remote system through SSH

* Launch job

* The Allinea Forge client will show the *Reverse Connect* request

Note:
> The request will specify:
* The host (typically a batch compute node) from where the request was made.
* The command line summary.

## Remote Launch Settings

The following configuration is required in the Allinea Forge client:

* Click on the *RemoteLaunch* drop down list

* Select *Configure...*

* Specify the following parameters:
  * Connection Name: An optional name for this connection
  * Host Name: The host name of the remote system you wish to connect to.
  Syntax:
  ```
  [username]@hostname[:port]
  ```
  To login via one or more intermediate hosts (e.g. a gateway), enter the sequence of host names separated by spaces, e.g.:
  ```
  gateway.mycompany.com frontend.mycompany.com
  ```

  * Remote Installation Directory: Product installation directory on the remote system
  Example:
  ```
  /opt/allinea/forge
  ```

  * Remote Script: Commands to be run remotely at start
  Example:
  ```
  module load forge
  ```
