# FAQs

{% hint style="warning" %}
🚧 Under Construction
{% endhint %}

If you have an issue that isn't addressed, please [contact us in Discord](https://discord.gg/uchaininfo) for troubleshooting.



<details>

<summary>Are there any additional tutorials for UCHAIN.INFO deployment?</summary>

We are currently working on more tutorials from the team. There are some user and community generated videos available here:\
\
[https://drive.google.com/drive/folders/1dH-ZxV1ShQ-cb7gObTXs3MtwSqVNJOqW ](https://drive.google.com/drive/folders/1dH-ZxV1ShQ-cb7gObTXs3MtwSqVNJOqW)

</details>

<details>

<summary>Where are the environmental variable (ENV) settings located?</summary>

* Frontend envs: [https://github.com/UdotCASH/uchain-info-frontend/blob/main/docs/ENVS.md](https://github.com/UdotCASH/uchain-info-frontend/blob/main/docs/ENVS.md)
* Backend envs: [https://github.com/UdotCASH/uchain-info//blob/master/docker-compose/envs/common-uchaininfo.env](https://github.com/UdotCASH/uchain-info//blob/master/docker-compose/envs/common-uchaininfo.env)

</details>

<details>

<summary>How do I connect the frontend to the backend?</summary>

This is done through the[ proxy setup](proxy-setup.md). Ensure you've added the `API_V2_ENABLED='true'` in your uchaininfo instance.

</details>

<details>

<summary>Do the frontend and backend need to be under the same domain?</summary>

Yes. Be sure to proxy all /api, /socket requests to the backend application and all other requests to the frontend. More info on the [proxy setup](proxy-setup.md) page.

</details>

<details>

<summary>What are the variable settings for a Polygon Edge client?</summary>

Be sure to set the following envs when setting up a Polygon Edge instance:

* `ETHEREUM_JSONRPC_VARIANT=geth`
* `INDEXER_INTERNAL_TRANSACTIONS_TRACER_TYPE=polygon_edge`

</details>

<details>

<summary>UCHAIN.INFO API/indexer is using 100% CPU, what should I do?</summary>

You should allocate more computing resources/cores to the instance.

</details>

<details>

<summary>What should I set the proxy_pass variable to?</summary>

Typically you should set to [http://youripaddress:your\_port\_number](http://youripaddress:8153/;). UCHAIN.INFO may not work if you set to [http://0.0.0.0:](http://0.0.0.0:8153/;)your\_port\_number

</details>
