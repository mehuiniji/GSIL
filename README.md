# GSIL(GitHub Sensitive Information Leak)

[中文文档](https://github.com/BlackHole1/GSIL/blob/master/README-zh.md)

> Monitor Github sensitive information leaks in near real time and send alert notifications.

## Installation

> Python3(Python2 is not tested)

```bash
$ git clone https://github.com/FeeiCN/gsil.git
$ cd gsil/
$ pip install -r requirements.txt
```

## Configuration

### gsil/config.gsil: Alarm mailbox and Guthub configuration

```conf
[mail]
host : smtp.exmail.qq.com
port : 25
mails : gsil@domain.com
from : GSIL
password : your_password
to : feei@feei.cn

[github]
# Whether the scanned data will be cloned to the local area immediately
clone: false

# Github Token
# https://github.com/settings/tokens
tokens : your_token
```

### gsil/rules.gsil: scanning rules

> Generally, The best rule is the characteristic code of the intranet(Example: mogujie's extranet is `mogujie.com`, intranet is `mogujie.org`. At this time, `mogujie.org` can be used as a rule)

> There are other similar code head characteristic code, external mailbox characteristic code, and so on

| field | meaning | optional | default | describe |
| --- | --- | --- | --- | --- |
| keyword | key word | required | - | When multiple keywords are used, space segmentation is used(Example: `'username password'`), When you need a precise search, use double(Example: `"quotesele.me"`) |
| ext | file suffix | optional | all suffixes | Multiple suffixes are separated by comma(Example: `java,php,python`) |
| mode |  matching mode | optional | normal-match | `normal-match`(The line that contains the keyword is matched, and the line near the line is matched) / `only-match`(Only the lines that match the key words7) / `full-match`(Not recommended for use)(The search results show the entire file)|

```
{
    # usually using the company name, used as the first parameter to open the scan(Example:`python gsil.py test`)
    "test": {
        # General use of product name
        "mogujie.com": {
            # Internal domain name of the company
            "\"mogujie.org\"": {},
            # Company code's characteristic code
            "copyright meili inc": {},
            # Internal host domain name
            "yewu1.db.mogujie.host": {},
            # External mailbox
            "mail.mogujie.com": {}
        }
    }
}
```

## Usage

```bash
$ python gsil.py test
```

```bash
$ crontab -e

# Run every hour
0 * * * * /usr/bin/python /var/app/gsil/gsil.py test > /tmp/gsil
# Send a statistical report at 11 p. m. every night
0 23 * * * /usr/bin/python /var/app/gsil/gsil.py --report
```
