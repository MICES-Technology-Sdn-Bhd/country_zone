Example seeder in Lavaral
```
    public function run()
    {
        $url = 'https://raw.githubusercontent.com/MICES-Technology-Sdn-Bhd/country_zone/master/country_zone.json';
        
        $client = new Client();
        $request = $client->get($url);
        
        $json = $request->getBody()->getContents();
        $countries = json_decode($json, true);
        
        foreach ($countries as $country) {
            $cnt = Country::firstOrCreate(
                [
                    'code' => $country['code']
                ],
                [
                    'name' => $country['name'],
                    'phone_code' => $country['phone_code']
                ]
            );

            if ($country['zones']) {
                foreach ($country['zones'] as $zone) {
                    $cnt->states()->firstOrCreate([
                        'name' => $zone['name']
                    ]);
                }
            }
        }
    }
```