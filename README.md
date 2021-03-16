# auto-report

Auto daily report for HITSZ Students.

## Usage

### Report Once

**Golang**

```shell
go run main.go -u your-studentID -p your-password -e your-email
```

**Docker**

```shell
docker run --rm rocketeerli/auto-report -u your-studentID -p your-password -e your-email
```

### Daily Auto Report 

**Action**

1. Fork this repository from here.

2. Add the repository secrets `STUDENTID`, `PASSWORD` and `EMAIL`  in your own repository's <a href="../../settings/secrets">Settings-Secrets</a>,  which also can be found by `Settings`-> `Secrets` -> `New repository secret `. (Repository secrets are invisible for others.)

3. Add `report.yml` file to `.github/workflows`, and write the following content to this file:

   ```yaml
   name: Auto Report
   on: 
     schedule:
       - cron: "30 3 * * *"
     push:
       branches: [main]
   
   env:
     - EMAIL: ${{ secrets.EMAIL }}
   
   jobs:
   
     report:
       name: auto report
       runs-on: ubuntu-latest
       steps:
       - uses: actions/checkout@v2
   
       - name: Set up Go
         uses: actions/setup-go@v2
         with:
           go-version: 1.15
   
       - name: Build
         run: go build -o auto-report .
           
       - name: Run
         run: |
           if [[ -z $EMAIL ]]
           then ./auto-report -u ${{ secrets.STUDENTID }} -p ${{ secrets.PASSWORD }}
           else ./auto-report -u ${{ secrets.STUDENTID }} -p ${{ secrets.PASSWORD }} -e ${{ secrets.EMAIL }}
           fi
   ```

   The time cron above is an UTC time, which is 8 hours slower than Beijing time zone.

   By default, this program will run at 11:30 a.m. everyday.

## License

auto-report is licensed by an MIT license as can be found in the LICENSE file.

