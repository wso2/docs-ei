# Changing Encrypted Secrets

Follow the steps given below to change an encrypted secrets in the `deployment.toml` file (stored in the `<MI_HOME>/conf` directory).

!!! Tip
    Note that this only applies to secrets in a VM deployment, which are encrypted using secure vault.

1. Be sure to shut down the server.
2. Open a command prompt and go to the `MI_HOME/bin/` directory where the cipher tool scripts (for Windows and Linux) are stored.
3. Execute the relevant command for your OS:

    ```bash tab='On Linux'
    ./ciphertool.sh -Dchange
    ```

    ```bash tab='On Windows'
    ./ciphertool.bat -Dchange
    ```

4. It will prompt for the primary keystore password. Enter the keystore password (which is `wso2carbon` for the default keystore).
5. The alias values of all the passwords that you encrypted will now be shown in a numbered list.
6. The system will then prompt you to select the alias of the password that you want to change. Enter the list number of the password alias.
7. The system will then prompt you (twice) to enter the new password. Enter your new password.
