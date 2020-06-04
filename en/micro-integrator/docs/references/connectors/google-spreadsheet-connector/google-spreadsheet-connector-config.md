# Google Spreadsheet Connector Reference

The following operations allow you to work with the Google Spreadsheet Connector. Click an operation name to see parameter details and samples on how to use it.

---

## Initialize the connector

To use the Google Spreadsheet connector, add the <googlespreadsheet.init> element in your proxy configuration before use any other Google Spreadsheet operations. The <googlespreadsheet.init> element is used to authenticate the user using OAuth2 authentication and allows the user to access the Google account which contains the spreadsheets. For more information on authorizing requests in Google Spreadsheets, see [https://developers.google.com/sheets/api/guides/authorizing](https://developers.google.com/sheets/api/guides/authorizing).

> **Note**: When trying it out the first time, you need to use valid accessToken to use the connector operations. If the provided accessToken has expired then the token refreshing flow will be handled inside the connector. See the [documetation to set up Google Spreadsheets and get credentials such as clientId, clientSecret, accessToken, and refreshToken](get-credentials-for-google-spreadsheet.md).

??? note "googlespreadsheet.init"
    The googlespreadsheet.init operation initializes the connector to interact with Google Spreadsheet.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>accessToken</td>
            <td>Access token which is obtained through the OAuth2 playground.</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>apiUrl</td>
            <td>The application URL of Google Sheet version v4. </td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>clientId</td>
            <td>Value of your client id, which can be obtained via Google developer console.</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>clientSecret</td>
            <td>Value of your client secret, which can be obtained via Google developer console.</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>refreshToken</td>
            <td>Refresh token which is obtained through the OAuth2 playground. It is used to refresh the accesstoken.</td>
            <td>Yes.</td>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <googlespreadsheet.init>
        <accessToken>{$ctx:accessToken}</accessToken>
        <clientId>{$ctx:clientId}</clientId>
        <clientSecret>{$ctx:clientSecret}</clientSecret>
        <refreshToken>{$ctx:refreshToken}</refreshToken>
        <apiUrl>{$ctx:apiUrl}</apiUrl>
    </googlespreadsheet.init>
    ```

    To get the OAuth access token directly call the init method (this method call getAccessTokenFromRefreshToken method itself) or add  <googlespreadsheet.getAccessTokenFromRefreshToken> element before <googlespreadsheet.init> element in your configuration.

    **Sample for getAccessTokenFromRefreshToken**

    ```xml
    <googlespreadsheet.getAccessTokenFromRefreshToken>
        <clientId>{$ctx:clientId}</clientId>
        <clientSecret>{$ctx:clientSecret}</clientSecret>
        <refreshToken>{$ctx:refreshToken}</refreshToken>
    </googlespreadsheet.getAccessTokenFromRefreshToken>
    ```

---

### Spreadsheet operation

??? note "googlespreadsheet.createSpreadsheet"
    This createSpreadsheet operation allows you to create a new spreadsheet by specifying the spreadsheet id, sheet properties, and add named ranges.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>spreadsheetId</td>
            <td>Unique value of the spreadsheet</td>
            <td>Optional.</td>
        </tr>
        <tr>
            <td>properties</td>
            <td>Properties of the spreadsheet.</td>
            <td>Optional.</td>
        </tr>
        <tr>
            <td>sheets</td>
            <td>List of sheets and their properties that you want to add into the spreadsheet. You can add multiple sheets.</td>
            <td>Optional.</td>
        </tr>
        <tr>
            <td>namedRanges</td>
            <td>Create names that refer to a single cell or a group of cells on the sheet. Following sample request will create name range with the name "Name" for  the range A1:A6.</td>
            <td>Optional.</td>
        </tr>
        <tr>
            <td>fields</td>
            <td>Specifying which fields to include in a partial response. For the following request only the "spreadsheetId" will be included in the response.</td>
            <td>Optional.</td>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <googlespreadsheet.createSpreadsheet>
        <spreadsheetId>{$ctx:spreadsheetId}</spreadsheetId>
        <properties>{$ctx:properties}</properties>
        <sheets>{$ctx:sheets}</sheets>
        <namedRanges>{$ctx:namedRanges}</namedRanges>
        <fields>{$ctx:fields}</fields>
    </googlespreadsheet.createSpreadsheet>
    ```

    **Sample request**

    The sample request given below calls the createSpreadsheet operation. With the following request we can specify spreadsheet details such as spreadsheet name ("Company"), sheet details such as sheet name ("Employees") as an array. So the spreadsheet will be created inside Google Sheets with the name "Company" and the sheet will be created with the name "Employees". Here we specify the "fields" property to get a partial response. As per the following request, only the "spreadsheetId" will be included in the response.

    ```json
    {
        "clientId":"xxxxxxxxxxxxxxxxxxxxxxxn6f2m.apps.googleusercontent.com",
        "clientSecret":"xxxxxxxxxxxxxxxxxxxxxxx",
        "refreshToken":"1/xxxxxxxxxxxxxx-fCyxRTyf-LpK6fDWF9DgcM",
        "accessToken":"ya29.xxxxxxxxxxxxxxxxxxxxxxxxxxxxx-pOuVvnbnHhkVn5u8t6Qr",
        "apiUrl":"https://sheets.googleapis.com/v4/spreadsheets",
        "properties":{
            "title": "Company"
        },
        "sheets":[
            {
            "properties":
                {
                    "title": "Employees"
                }
            }
        ],
        "fields": "spreadsheetId"
    }
    ```

    **Sample response**

    ```json
    {
        "spreadsheetId": "1bWbo72MAhKgeNDCPcE4Wj3uGgN7K9lW1ckDScZV8b30"
    }
    ```

---

### Sheet operations

??? note "googlespreadsheet.addSheetBatchRequest"
    The addSheetBatchRequest operation allows you to add new sheets to an existing spreadsheet. You can specify the sheet properties for the new sheet. An error is thrown if you provide a title that is used for an existing sheet. For more information, see [the Google Spreadsheet documentation](https://developers.google.com/sheets/reference/rest/v4/spreadsheets/request#AddSheetRequest).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>spreadsheetId</td>
            <td>Unique value of the spreadsheet</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>requests</td>
            <td>It contains data that is an update to apply to a spreadsheet. To add multiple sheets within the spread sheet, need to repeat "addSheetBatchRequest" property within the requests attribute as below.</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>fields</td>
            <td>Specifying which fields to include in a partial response. For the following request only the "spreadsheetId" will be included in the response.</td>
            <td>Optional.</td>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <googlespreadsheet.addSheetBatchRequest>
        <spreadsheetId>{$ctx:spreadsheetId}</spreadsheetId>
        <requests>{$ctx:requests}</requests>
        <fields>{$ctx:fields}</fields>
    </googlespreadsheet.addSheetBatchRequest>
    ```

    **Sample request**

    The sample request given below calls the addSheetBatchRequest operation. The request specifies the multiple sheet properties, such as the sheet name ("Expenses1", "Expenses2"), sheet type ("GRID"), and the dimension ((50,10), (70,10)) of the sheet as an array. The fields property is specified to get a partial response. The spreadsheetId and replies values will be included in the response. replies contain properties such as sheet name, type, row, column count, and sheetId.

    ```json
    {
        "clientId":"617729022812-xxxxxxxxxx.apps.googleusercontent.com",
        "clientSecret":"xxxxxxxxxxxxxxxxx",
        "refreshToken":"1/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx-fCyxRTyf-LpK6fDWF9DgcM",
        "accessToken":"ya29.xxxxxxxxxxxxxxxxxxxxxxxxxx-pOuVvnbnHhkVn5u8t6Qr",
        "apiUrl":"https://sheets.googleapis.com/v4/spreadsheets",
        "spreadsheetId": "1oGxpE3C_2elS4kcCZaB3JqVMiXCYLamC1CXZOgBzy9A",
        "requests": [
        {
            "addSheet": {
                "properties": {
                    "title": "Expenses1",
                    "sheetType": "GRID",
                    "gridProperties": {
                        "rowCount": 50,
                        "columnCount": 10
                    }
                }
            }
        },
        {
            "addSheet": {
                "properties": {
                    "title": "Expenses2",
                    "sheetType": "GRID",
                    "gridProperties": {
                        "rowCount": 70,
                        "columnCount": 10
                    }
                }
            }
        }
        ],
        "fields": "spreadsheetId,replies"
    }
    ```

    **Sample response**

    ```json
    {
        "spreadsheetId": "1oGxpE3C_2elS4kcCZaB3JqVMiXCYLamC1CXZOgBzy9A",
        "replies": [
            {
                "addSheet": {
                    "properties": {
                        "sheetId": 372552230,
                        "title": "Expenses1",
                        "index": 1,
                        "sheetType": "GRID",
                        "gridProperties": {
                            "rowCount": 50,
                            "columnCount": 10
                        }
                    }
                }
            },
            {
                "addSheet": {
                    "properties": {
                        "sheetId": 568417391,
                        "title": "Expenses2",
                        "index": 2,
                        "sheetType": "GRID",
                        "gridProperties": {
                            "rowCount": 70,
                            "columnCount": 10
                        }
                    }
                }
            }
        ]
    }
    ```

??? note "googlespreadsheet.deleteSheetBatchRequest"
    The deleteSheetBatchRequest operation allows you to remove sheets from a given spreadsheet using "sheetId". You can get the "sheetId" using the getSheetMetaData operation. For more information, see [the Google Spreadsheet documentation](https://developers.google.com/sheets/reference/rest/v4/spreadsheets/request#deletesheetrequest).
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>spreadsheetId</td>
            <td>Unique value of the spreadsheet</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>requests</td>
            <td>It contains data that is an update to apply to a spreadsheet. To add multiple sheets within the spread sheet, need to repeat "addSheetBatchRequest" property within the requests attribute as below.</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>fields</td>
            <td>Specifying which fields to include in a partial response. For the following request only the "spreadsheetId" will be included in the response.</td>
            <td>Optional.</td>
        </tr>
    </table>

    **Sample configurations**

    ```xml
    <googlespreadsheet.deleteSheetBatchRequest>
        <spreadsheetId>{$ctx:spreadsheetId}</spreadsheetId>
        <requests>{$ctx:requests}</requests>
        <fields>{$ctx:fields}</fields>
    </googlespreadsheet.deleteSheetBatchRequest>
    ```

    **Sample request**

    ```json
    {
        "clientId":"617729022812-xxxxxxxxxxxxxxxxxxxxx.apps.googleusercontent.com",
        "clientSecret":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "refreshToken":"1/xxxxxxxxxxxxxxxxxxxxxxx-fCyxRTyf-LpK6fDWF9DgcM",
        "accessToken":"ya29.xxxxxxxxxxxxxxxxxxxxxxxxxxxxx-pOuVvnbnHhkVn5u8t6Qr",
        "apiUrl":"https://sheets.googleapis.com/v4/spreadsheets",
        "spreadsheetId": "12KoqoxmxxxxxxxxxxxxxxxxxxxxxKMEIFGCD9EBdrXFGA",
        "requests": [
        {
            "deleteSheet":
            {
                "sheetId": 813171540
            }
        }
        ],
        "fields": "spreadsheetId"
    }
    ```

    **Sample response**

    ```json
    {
        "spreadsheetId": "12KoqoxmykLLYbtsm6CEOggk5bTKMEIFGCD9EBdrXFGA"
    }
    ```