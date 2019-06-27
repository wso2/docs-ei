# Adding Third Party Non OSGi Libraries

WSO2 SP is a OSGi-based product. Therefore, when you integrate third
pary products such as Oracle with WSO2 SP, you need to check whether the
libraries you need to add to WSO2 SP are OSGi based. If they are not,
you need to convert them to OSGi bundles before adding them to the
`         <SP_HOME>/lib        ` directory.

To convert jar files to OSGi bundles, follow the procedure given below:

1.  Download the non-OSGi jar for the required third party product, and
    save it in a preferred directory in your machine.
2.  In your CLI, navigate to the \<SP\_HOME\>/bin directory. Then issue
    the following command.  
    `          ./jartobundle.sh <PATH_TO_NON-OSGi_JAR> ../lib         `  
      
    This generates the converted file in the
    `          <SP_HOME>/lib         ` directory.
3.  Restart the WSO2 SP server.
