# check_awx_jobs

`check_awx_jobs` is a icinga2 plugin that can be used to check the status of job(s) in [AWX](https://github.com/ansibl/awx).

## Usage

This plugin makes use of token authentication against the API of AWX. To create a token, please follow [this](https://docs.ansible.com/ansible-tower/latest/html/administration/oauth2_token_auth.html) documentation.

## Arguments

| Argument |     Description           	 	    	| Required |
|----------|----------------------------------------|----------|
| --host   |  The FQDN of the AWX host.				| Yes	   |
| --jobs   |  Which job ID's to check. If you want to check multiple jobs, then put the ID's comma-separated.				| Yes 	   |
| --token  |  Token to authenticate against AWX 	| Yes	   |
| --verify |  Whether to verify the HTTPS connection| No	   |

## Exit states

The plugin will only go in `CRITICAL` state when the last run of a job resulted into a failure.

## Examples

icinga2 `CheckCommand` definition:
```
object CheckCommand "check_awx_jobs" {
    import "plugin-check-command"

    command = [ PluginDir + "/check_awx_jobs" ]
    arguments = {
        "--host" = { value = "$awx_host$" }
        "--jobs" = { value = "$awx_jobs$" }
        "--token" = { value = "$awx_token$" }
    }   
}
```

Variable definition:
```ini
awx_host = "http://awx.foobar"
awx_jobs = 1 # If you want to check multiple jobs, then put the job ids in a comma separated format: 1,3,3,7
awx_token = "verisekjoertoken"
```

If you think the code can be improved, then dont hesitate to open a PR! :)
