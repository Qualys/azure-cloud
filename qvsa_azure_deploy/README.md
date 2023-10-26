<h2>Deploy Qualys Virtual Scanner Appliance On Azure</h2>
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FQualys%2Fazure-cloud%2Fmaster%2Fqvsa_azure_deploy%2Fazure_deploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

<h3>ARM template files</h3>
<table style="width:50%">
  <tr>
    <th>ARM template file</th>
    <th>platform</th>
  </tr>
  <tr>
    <td>azure_deploy.json</td>
    <td>Azure (portal.azure.com)</td>
  </tr>
  <tr>
    <td>azure_stack_deploy.json</td>
    <td>Azure Stack (Private/hybrid)</td>
  </tr>
</table>

<h3>Deploy using azcli</h3>

``` shell
# to pass 'proxy' as parameter use parameter-file
# see parameter file examples in 'example_parameters' directory
az group deployment create --debug --verbose --template-file azure_deploy.json --resource-group resource-group-name

# for Azure Stack
az group deployment create --debug --verbose --template-file azure_stack_deploy.json --resource-group resource-group-name
```
``` shell
# type '?' to see help text
Please provide string value for 'perscode' (? for help): ?
Input: Personalization code
Description: Qualys scanner personalization code, exactly 14 digits
Learn more: https://discussions.qualys.com/docs/DOC-5725-scanning-in-microsoft-azure-using-resource-manager-arm
```
``` shell
# with parameter file
az deployment group create --debug --verbose --template-file azure_deploy.json --resource-group resource-group-name --parameters path_to_json_parameter_file
```
<h3>Deploy using PowerShell</h3>

``` shell
New-AzResourceGroupDeployment -ResourceGroupName resource-group-name -TemplateFile azure_deploy.json -TemplateParameterFile path_to_json_parameter_file
```
<h3>Parameters</h3>
<table style="width:100%">
  <tr>
    <th>Parameter</th>
    <th>Input Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>location</td>
    <td>Location identifier</td>
    <td>Location identifier for deployment (example:- eastus)<br><a href="https://azure.microsoft.com/en-in/global-infrastructure/locations" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>scannerName</td>
    <td>Scanner VM name</td>
    <td>VM name on Azure can be 1-64 character long and may contain alphanumerics, underscores, periods, and hyphens.<br> <a href="https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/resource-name-rules#microsoftcompute" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>scannerVmSize</td>
    <td>Scanner VM size</td>
    <td>Any one of the following<br><code>Standard_A1_v2, Standard_A2_v2, Standard_A4_v2, Standard_A8_v2, Standard_B1ms, Standard_B2s, Standard_B2ms, Standard_B4ms, Standard_D2a_v4, Standard_D4a_v4, Standard_D2as_v4, Standard_D4as_v4, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_DS1_v2, Standard_DS2_v2, Standard_DS3_v2, Standard_D2_v3, Standard_D4_v3, Standard_D2s_v3, Standard_D4s_v3, Standard_F2s_v2, Standard_F4s_v2, Standard_D11_v2, Standard_DS11_v2, Standard_E2a_v4, Standard_E2as_v4, Standard_E2_v3, Standard_E2s_v3</code><br><a href="https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>perscode</td>
    <td>Personalization code</td>
    <td>Qualys scanner personalization code, exactly 14 digits.<br><a href="https://discussions.qualys.com/docs/DOC-5725-scanning-in-microsoft-azure-using-resource-manager-arm" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>proxy</td>
    <td>Proxy string</td>
    <td>Optional proxy configuration for scanner.<br>Sample formats-<br><code>proxy://&lt;host&gt;:&lt;port&gt; (No auth proxy)</code><br><code>proxy://&lt;user&gt;:&lt;password&gt;@&lt;host&gt;:&lt;port&gt; (Auth proxy)</code><br><code>proxy://&lt;domain\\user&gt;:&lt;password&gt;@&lt;host&gt;:&lt;port&gt; (Auth proxy with domain user)</code><br><a href="https://discussions.qualys.com/docs/DOC-5725-scanning-in-microsoft-azure-using-resource-manager-arm" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>ImageResourceIdOrVhdUri</td>
    <td>
      <ol>
        <li>qVSA image resource id</li>
        <li>Storage account vhd blob</li>
        <li>'global_marketplace'</li>
      </ol>
    </td>
    <td>
      <ol>
        <li>Deploy qVSA scanner from existing image</li>
        <li>Storage account uri to qVSA disk vhd blob</li>
        <li>String identifier to deploy scanner from global marketplace image</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>osDiskType</td>
    <td>OS disk type from allowed values</td>
    <td> Any one of the following-<br><code>Premium_LRS, StandardSSD_LRS, Standard_LRS</code><br>Premium Disk (Premium_SSD) is recommended due to their production performance but only available with selected VM sizes.<br><a href="https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disks-types" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>bootDiagStorageAccNameOrUri</td>
    <td>
      <ol>
        <li>Unique name for new storage account</li>
        <li>Existing storage account uri</li>
        <li>Leave it empty</li>
      </ol>
    </td>
    <td>
      <ol>
        <li>Ensure that name string is unique as storage account names are globally unique</li>
        <li>Template will use existing storage account</li>
        <li>Template will disable boot diagnostics (Not recommended practice)</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>virtualNetworkNewOrExisting</td>
    <td>
      <ol>
        <li>'new' for new virtual network with default configurations</li>
        <li>resource_id:subnet_name where resource_id is of the existing virtual network</li>
      </ol>
    </td>
    <td>Virtual network options for scanner vm. If no value provided then template will create a new vNET as in input 'new'. To link scanner with existing virtual network put resource_id:subnet_name where resource_id is of existing vNet and subnet_name is name of existing subnet in the vNet.</td>
  </tr>
  <tr>
    <td>publicIpNewOrExisting</td>
    <td>
      <ol>
        <li>'new'</li>
        <li>resource_id of existing dissociated public ip instance</li>
        <li>Leave it empty</li>
      </ol>
    </td>
    <td>
      <ol>
        <li>Template will create new Public IP instance with default configuration parameters</li>
        <li>Template will link existing Public IP instance to VM</li>
        <li>Template will skip Public IP configuration</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>newVirtualNetworkCustomObject</td>
    <td>vNet custom property object</td>
    <td>Values in this object will override default values. See <a href="#custom-virtual-network-and-public-ip-parameters">Custom Virtual Network And Public IP Parameters</a> section for more information.</td>
  </tr>
  <tr>
    <td>newPublicIpCustomObject</td>
    <td>Public IP custom property object</td>
    <td>Values in this object will override default values. See <a href="#custom-virtual-network-and-public-ip-parameters">Custom Virtual Network And Public IP Parameters</a> section for more information.</td>
  </tr>
  <tr>
    <td>bootDiagStrorageAccDomain<br><code>(azure_stack_deploy.json)</code></td>
    <td>Storage account domain<br><code>(Only for Azure Stack azure_stack_deploy.json)</code></td>
    <td>Storage account domain for boot diagnostics storage account. If empty string provided then template will use Azure Stack Development Kit (ASDK) platform's default domain <code>blob.local.azurestack.external</code> as default. This parameter is not required and will be ignored if using existing storage account URI in bootDiagStorageAccNameOrUri parameter.<br>For example in URI-<br><code>https://qvsaimages.blob.local.stackserver.com/images/qVSA-Azure-2.7.29-1.vhd</code><br><code>blob.local.stackserver.com</code> is the storage account domain.</td>
  </tr>
  <tr>
    <td>virtualNetworkName<br><code>(azure_deploy.json)</code></td>
    <td>Name of the virtual network instance<br><code>(Only for Azure azure_deploy.json)</code></td>
    <td>This parameter can be used to provide a custom name when creating a new virtual network for the scanner VM during deployment. This parameter is used in conjunction with virtualNetworkNewOrExisting==new. If not provided, the ARM template will come up with a name by combining the "scannerName" and a random string. You may use this parameter in case your organization has an established resource naming policy.</td>
  </tr>
  <tr>
    <td>networkInterfaceName<br><code>(azure_deploy.json)</code></td>
    <td>Name of the network interface instance<br><code>(Only for Azure azure_deploy.json)</code></td>
    <td>Use this parameter to provide a custom name for the network interface instance created for the scanner VM. If not provided, the ARM template will come-up with a name by combining the "scannerName" and a random string. You may use this parameter in-case your organization has an established resource naming policy.</td>
  </tr>
  <tr>
    <td>nsgName<br><code>(azure_deploy.json)</code></td>
    <td>Name of the network security group instance<br><code>(Only for Azure azure_deploy.json)</code></td>
    <td>Use this parameter to provide a custom name for the network security group instance created for the scanner VM. If not provided, the ARM template will come-up with a name by combining the "scannerName" and a random string. You may use this parameter in-case your organization has an established resource naming policy.</td>
  </tr>
  <tr>
    <td>publicIpName<br><code>(azure_deploy.json)</code></td>
    <td>Name of the public IP instance<br><code>(Only for Azure azure_deploy.json)</code></td>
    <td>This parameter can be used to provide a custom name when creating a new publicIP for the scanner VM during deployment. This parameter is used in conjunction with publicIpNewOrExisting==new. If not provided, the ARM template will come-up with a name by combining the "scannerName" and a random string. You may use this parameter in case your organization has an established resource naming policy.</td>
  </tr>
</table>
<h3>Custom Virtual Network And Public IP Parameters</h3>
<p>Template deploys new virtual network and public ip instances with default paramters mentioned inside template. If needed these variables can be overridden with custom values. Paramters newVirtualNetworkCustomObject and newPublicIpCustomObject do exactly that. Each accepts a custom json object representing vNET and Public-IP configuration respectively.</p>

<p>Example parameter file for more details: <a href="https://github.com/Qualys/azure-cloud/blob/master/qvsa_azure_deploy/example_parameters/custom_vNet_and_pubip_param.json" target="_blank">example_parameters/custom_vNet_and_pubip_param.json</a></p>

``` json
{
  "vNetAddressPrefixes": ["10.0.0.0/24"],
  "subnet": {
    "name": "scanner-subnet",
    "addressPrefix": "10.0.0.0/24"
  }
}
```
``` json
{
  "sku": "Basic",
  "publicIpAllocationMethod": "Dynamic",
  "idleTimeoutInMinutes": 30
}
```
<h3>Parameter File Examples</h3>
<ol>
  <li>Basic Example: Easy deployment <a href="https://github.com/Qualys/azure-cloud/blob/master/qvsa_azure_deploy/example_parameters/deploy_all_resources_with_defaults.json" target="_blank">example_parameters/deploy_all_resources_with_defaults.json</a></li>
  <li>Deploy from global market image: <a href="https://github.com/Qualys/azure-cloud/blob/master/qvsa_azure_deploy/example_parameters/deploy_from_global_marketplace_image.json" target="_blank">example_parameters/deploy_from_global_marketplace_image.json.json</a></li>
  <li>Custom vNET and Public IP parameters: See <a href="#custom-virtual-network-and-public-ip-parameters">Custom Virtual Network And Public IP Parameters</a> section for more information. <a href="https://github.com/Qualys/azure-cloud/blob/master/qvsa_azure_deploy/example_parameters/custom_vNet_and_pubip_param.json" target="_blank">example_parameters/custom_vNet_and_pubip_param.json</a></li>
  <li>No Public IP and disable boot diagnostics: <a href="https://github.com/Qualys/azure-cloud/blob/master/qvsa_azure_deploy/example_parameters/disable_boot_diag_and_no_public_ip.json" target="_blank">example_parameters/disable_boot_diag_and_no_public_ip.json</a></li>
  <li>Link scanner to already existing vNET and Public IP: Deploy scanner from image resource_id <a href="https://github.com/Qualys/azure-cloud/blob/master/qvsa_azure_deploy/example_parameters/existing_stracc_image_vNet_publicip.json" target="_blank">example_parameters/existing_stracc_image_vNet_publicip.json</a></li>
  <li>Provide custom names for virtual network, network interface, network security group and public IP instances: <a href="https://github.com/Qualys/azure-cloud/blob/master/qvsa_azure_deploy/example_parameters/custom_resource_names.json" target="_blank">example_parameters/custom_resource_names.json</a></li>
  <li>Deploy scanner on Azure Stack: <a href="https://github.com/Qualys/azure-cloud/blob/master/qvsa_azure_deploy/example_parameters/azure_stack.json" target="_blank">example_parameters/azure_stack.json</a></li>
</ol>

<h3>Accepting Legal Policy</h3>
<p>If you are deploying the Qualys Virtual Scanner Appliance for the first time on your subscription, Microsoft may require you to accept our legal terms and conditions first before deploying VM resources. You can view our legal terms and conditions and privacy policy using the following command.</p>

```shell
az vm image terms show --offer 'qualys-virtual-scanner' --plan 'qvsa' --publisher 'qualysguard'

Output-
{
  "accepted": false,
  "id": "/subscriptions/n112ug51-7e18-48u9-9b0a-9bnbv46545r20/providers/Microsoft.MarketplaceOrdering/offerTypes/VirtualMachine/publishers/qualysguard/offers/qualys-virtual-scanner/plans/qvsa-app/agreements/current",
  "licenseTextLink": "https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_QUALYSGUARD%253a24QUALYS%253a2DVIRTUAL%253a2DSCANNER%253a24QVSA%253a2DAPP%253a247X6MWRP2X774O53E7DCCZFOLE2LAAOS6V3PWH3SELAOUCBDJJELHBXCU4VK55ZVSCBRCMWFBIL4DCLAHSWNHXFG2ASI5CJ2Y4WGCPYA.txt",
  "name": "qvsa-app",
  "plan": "qvsa-app",
  "privacyPolicyLink": "https://www.qualys.com/company/privacy/",
  "product": "qualys-virtual-scanner",
  "publisher": "qualysguard",
  "retrieveDatetime": "2020-07-23T09:10:56.7986203Z",
  "signature": "BVFZEH74ZTBQY6TKG6JEDM7STMZHWGU7DCSR3DRBP4MQLWAJ2HNC7V47PQSPGBA7AMRDJUG46OGITJP3WHA4333PQACD3IIBNRZ3MZA",
  "type": "Microsoft.MarketplaceOrdering/offertypes"
}
```

<p>With the following command, you can accept our legal terms and conditions.</p>

```shell
az vm image terms accept --offer 'qualys-virtual-scanner' --plan 'qvsa' --publisher 'qualysguard'

Output-
{
  "accepted": true,
  "id": "/subscriptions/n112ug51-7e18-48u9-9b0a-9bnbv46545r20/providers/Microsoft.MarketplaceOrdering/offerTypes/Microsoft.MarketplaceOrdering/offertypes/publishers/qualysguard/offers/qualys-virtual-scanner/plans/qvsa/agreements/current",
  "licenseTextLink": "https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_QUALYSGUARD%253a24QUALYS%253a2DVIRTUAL%253a2DSCANNER%253a24QVSA%253a247X6MWRP2X774O53E7DCCZFOLE2LAAOS6V3PWH3SELAOUCBDJJELHBXCU4VK55ZVSCBRCMWFBIL4DCLAHSWNHXFG2ASI5CJ2Y4WGCPYA.txt",
  "name": "qvsa",
  "plan": "qvsa",
  "privacyPolicyLink": "https://www.qualys.com/company/privacy/",
  "product": "qualys-virtual-scanner",
  "publisher": "qualysguard",
  "retrieveDatetime": "2020-07-23T09:19:07.8599072Z",
  "signature": "ONEMMSJA5ORJCRED26NMZ6YWXOH3GWVRNNSQBQ3HPV2HWMRDDGRR5IN5N3LP3EYBF3UG5A5ZKK2OJMS5S2RPA4UYV55Q23V4P5EY7RA",
  "type": "Microsoft.MarketplaceOrdering/offertypes"
}
```
