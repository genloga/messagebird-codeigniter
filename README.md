# messagebird-codeigniter
This small repo will help you to understand of to integrate the messagebird api into a codeigniter project and use all the functions avalaible.

#1- First step you just need to download the php library you would like to integrate to your project 
which could be found here https://github.com/messagebird/php-rest-api/archive/master.zip

#2- Once the library downloaded you place it under the application/libraries folder of your codeigniter project

#3- Now you need to configure it under config/autoload.php file like this 
$autoload['packages'] = array(APPPATH.'libraries','MessageBrid/autoload');

#4- The final step is to call it in any controller you would like to use it in 
require_once(APPPATH . '/libraries/MessageBird/autoload.php');

EXAMPLE :

public function sendSMSMessageBird(){

        $MessageBird = new \MessageBird\Client(API_KEY); // Set your own API access key here.

        $Message             = new \MessageBird\Objects\Message();
        $Message->originator = $this->input->post('soa');;
        $Message->recipients = $this->input->post('telephone');
        $Message->body       = $this->input->post('message');

        try {
            $MessageResult = $MessageBird->messages->create($Message);
            //var_dump($MessageResult);
            //print_r($Message->recipients);

        } catch (\MessageBird\Exceptions\AuthenticateException $e) {
            // That means that your accessKey is unknown
            $data = array(
                'code' => 'error',
                'message' => 'Your api key is incorrect '
            );

        } catch (\MessageBird\Exceptions\BalanceException $e) {
            // That means that you are out of credits, so do something about it.
            $data = array(
                'code' => 'error',
                'message' => 'Not enough balance!'
            );
            $this->smsIndividuel($data);

        } catch (\Exception $e) {
            $data = array(
                'code' => 'error',
                'message' => $e->getMessage()
            );
        }

#5- That's it. Good luck!