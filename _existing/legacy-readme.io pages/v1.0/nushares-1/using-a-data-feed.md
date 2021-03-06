#About data feeds

Data feeds let users configure their Nu client to pull voting data from the internet. All that is needed is a URL to the voting data. You can use the **setdatafeed** RPC call to configure the feed from the command line. The **getdatafeed** RPC can be used to view the current feed. The client pulls the data every 60 seconds by default, but this can be changed using the **datafeeddelay** configuration (for example you can add `datafeeddelay=30` in your nu.conf). The data feed information is stored in your NuShares wallet.dat file.

Your previous votes in the client are replaced when you set a data feed. If votes are manually added after you set the feed they will be removed on the next pull. You optionally can choose to ignore individual parts of a voting feed (motions, custodians, or park rates). If you choose to ignore custodian votes from the feed, for example, you may manually edit custodian votes in the client without the data feed overwriting them. All parts are selected by default.

#Adding a data feed

The data feed button can be found at the bottom of the NuShares voting page
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/zCp64E1wS1iARu59xDPf",
        "Capture.PNG",
        "852",
        "531",
        "#d83b16",
        ""
      ]
    }
  ]
}
[/block]
Pressing the data feed button will open up the feed dialog box. From here you can provide the data feed URL, and select which parts of the voting data you would like your client to pull down.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/E9Re15F3SbOvdSa5pRMN",
        "Capture.PNG",
        "403",
        "434",
        "#1f3f70",
        ""
      ]
    }
  ]
}
[/block]


##URL Signature

In the screen shot above you'll notice two extra fields. To prevent man-in-the-middle attacks data feed sources may also provide a secondary URL that points to a signature of the voting data. They will also provide the NuShares address used to sign it. These two additional pieces of information can help assure that votes have not changed while traveling to the client. The client will display an error if a change has been noticed and the feed data will be ignored. The URL signature is not required to use data feeds, but it is much more secure.

##votenotify configuration

It’s still very important for users to be aware of what they’re voting for when using a feed. The vote data provided by the data feed is always recorded in debug.log (but the file is truncated regularly). In addition the **votenotify** configuration is available. Providing this configuration the path to a script in the [nu.conf file](http://docs.nubits.com/v1.0/docs/creating-conf-file) will invoke that script when votes are changed. This could be used to trigger a script that sends an email, or creates a file with a diff of the changes for review.

Example nu.conf
[block:code]
{
  "codes": [
    {
      "code": "testnet=1\nserver=1\nvotenotify=C:\\folder\\folder\\script.bat",
      "language": "shell"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Using data feeds from the daemon"
}
[/block]
#Setting the data feed
    
[block:code]
{
  "codes": [
    {
      "code": "nud setdatafeed https://abc.com/my-datafeed.json",
      "language": "text"
    }
  ]
}
[/block]
#Setting up a signed data feed
[block:code]
{
  "codes": [
    {
      "code": "nud setdatafeed https://abc.com/my-datafeed.json https://abc.com/my-datafeed.json.signature SSPwKhtFEyQvUx9cPjaC8yUxdeNrTMfJfF",
      "language": "text"
    }
  ]
}
[/block]
#Using only specific parts of a data feed (custodians, park rates, motions)
[block:code]
{
  "codes": [
    {
      "code": "nud setdatafeed https://abc.com/my-datafeed.json https://abc.com/my-datafeed.json.signature SSPwKhtFEyQvUx9cPjaC8yUxdeNrTMfJfF custodians,parkrates",
      "language": "text"
    }
  ]
}
[/block]
#Or without signature
[block:code]
{
  "codes": [
    {
      "code": "nud setdatafeed https://abc.com/my-datafeed.json \"\" \"\" custodians,parkrates",
      "language": "text"
    }
  ]
}
[/block]
#To verify your current settings:
[block:code]
{
  "codes": [
    {
      "code": "nud getdatafeed",
      "language": "text"
    }
  ]
}
[/block]