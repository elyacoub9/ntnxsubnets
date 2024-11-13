# ntnxsubnets
bash script to create multiple subnets 
- you will need a `csv` file in this format: `VlanName;VLAN-ID`
example:
```csv
BDA_Application;1102
BDA_DMZ_PARTNERS;1140
```
name it `subnets.csv`

- run these commands in a CVM
1) create the script `ntnxsubnets` :

```bash
cat <<EOF > ntnxsubnets
#!/bin/bash

subnets="\$1"
counter=0

while IFS=';' read -r vlan_name vlan_id; do

        acli net.create "\$vlan_name" vlan="\$vlan_id" virtual_switch=vs1";
        if [ \$? -eq 0 ]; then
                echo "\$vlan_name created successfully"
                counter=\$((counter+1))
        else
                echo "\$vlan_name not created"
        fi

done < "\$subnets"

echo "--------------------------"
echo "subnets created = \$counter"
echo "--------------------------"
```

2) make it executable:
```bash
chmod u+x ntnxsubnet
```

3) run the script:
```bash
./ntnxsubnet subnets.csv
```


