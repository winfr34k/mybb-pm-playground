# Private Messages (PMs) in MyBB

**Table:** `PREFIX_privatemessage`

Contains all private messages, regardless of sender.

## Fields

| Name       | Type          | Default | Description                                              |
|------------|---------------|---------|----------------------------------------------------------|
| pmid       | int           | *None*  | AUTO\_INCREMENT ID                                       |
| uid        | int           | 0       | FK to user a PM belongs to                               |
| toid       | int           | 0       | FK Recipient User ID                                     |
| fromid     | int           | 0       | FK Sender User ID                                        |
| recipients | text          | *None*  | Original `recipients` line                               |
| folder     | smallint      | 1       | Folder where the PM should reside in                     |
| subject    | varchar(120)  | *Empty* | Message subject                                          |
| icon       | int           | 0       | Icon to use for the PM                                   |
| message    | text          | *None*  | Message body                                             |
| dateline   | int           | 0       | UNIX timestamp ???                                       |
| deletetime | int           | 0       | UNIX timestamp when the message was deleted              |
| status     | int           | 0       | Message status, see below                                |
| statustime | int           | 0       | UNIX timestamp when the last status change happened      |
| includesig | bool          | false   | Whether to include the user's signature or not           |
| smilieoff  | bool          | false   | Whether to use smily parsing in the PM or not            |
| receipt    | bool          | false   | Whether to get notified when the message was read or not |
| readtime   | int           | 0       | UNIX timestamp when the message was first read           |
| ipaddress  | varbinary(16) | *Empty* | MyBB's packed IP representation for the sender           |

**Note:** Fields without a default are mandatory.

If PMs have multiple recipients, they get duplicated accordingly.

### Status

This is a pseudo-enum, spread over two files:

| ID | Meaning        |
|----|----------------|
| 0  | New, Unread    |
| 1  | Read           |
| 2  | *Unused*       |
| 3  | Got replied to |
| 4  | Got forwaded   |

ID 2 is not in use anywhere in the code:

```shell
$ grep -ir "'status' => 2"
./wwwroot/inc/tasks/massmail.php:			$db->update_query("massemails", array('status' => 2), "mid='{$mass_email['mid']}'");
```

This result seems unrelated.

## Relevant for our conversion

At the moment, the WBB converter takes all PMs and just treats them all as new. While we can figure out all
relevant PMs (where status = 3), we cannot simply find out where the reply went, except for heuristics:

**TODO**

