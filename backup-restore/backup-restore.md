Create and configure the snapshot directory.

Using the Secure Shell (SSH), log in to the node-1 node as cloud_user via the public IP address.

Become the elastic user with:
sudo su - elastic

Create the directory where snapshots will be stored with:
mkdir /home/elastic/snapshot

Add the following line to /home/elastic/elasticsearch/config/elasticsearch.yml:
path.repo: "/home/elastic/snapshots"

Restart the elasticsearch node with:
pkill -F /home/elastic/elasticsearch/pid
/home/elastic/elasticsearch/bin/elasticsearch -d -p pid

Create the "test_repo" repository.

Use the Kibana console tool to execute the following:
PUT _snapshot/test_repo
{
  "type": "fs",
  "settings": {
    "location": "/home/elastic/snapshots"
  }
}

Back up the "bank" index.

Use the Kibana console tool to execute the following:
PUT _snapshot/test_repo/bank_1?wait_for_completion=true
{
  "indices": "bank", 
  "include_global_state": false
}

Restore the "bank" index as "bank_restored".

Use the Kibana console tool to execute the following:
POST _snapshot/test_repo/bank_1/_restore
{
  "indices": "bank",
  "rename_pattern": "(.+)",
  "rename_replacement": "$1_restored"
}
