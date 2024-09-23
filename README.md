import json
import logging
import pathlib

def valid_slot_welcome(slots):

    if not slots['WelcomeMessage'] or not slots['WelcomeMessage'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'WelcomeMessage'
        }
    
    return {'isValid': True}

def valid_slot_schedule_appointment(slots):
        
    if not slots['CustomerName'] or not slots['CustomerName'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'CustomerName'
        }
        
    if not slots['DateOfBirth'] or not slots['DateOfBirth'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'DateOfBirth'
        }
        
    if not slots['PostalCode'] or not slots['PostalCode'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'PostalCode'
        }
        
    if not slots['CenterOption'] or not slots['CenterOption'].get('value', {}).get('interpretedValue', ''):
        return {
            
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'CenterOption'
        }
        
    # if slots['CenterOption']['value']['interpretedValue'] == "Monroe":
    #     return {
    #         'isValid': False,
    #         'incorrectOption': False,
    #         'monroe': True,
    #         'violatedSlot': 'CenterOption'
    #     }
        
    if not slots['CenterConfirmation'] or not slots['CenterConfirmation'].get('value', {}).get('interpretedValue', ''):
        return {
            
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'CenterConfirmation'
        }
        
    # if slots['CenterConfirmation']['value']['interpretedValue'] == 'Change Location':
    #     return {
    #         'isValid': False,
    #         'incorrectOption': False,
    #         'fallback': True,
    #         'violatedSlot': 'CenterOption'
    #     }
        
    if not slots['InsuranceOption'] or not slots['InsuranceOption'].get('value', {}).get('interpretedValue', ''):
        return {
            
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'InsuranceOption'
        }
        
    # if not slots['TypeOfInsurance'] or not slots['TypeOfInsurance'].get('value', {}).get('interpretedValue', ''):
    #     return {
    #         'isValid': False,
    #         'incorrectOption': False,
    #         'violatedSlot': 'TypeOfInsurance'
    #     }
    
    # if slots['TypeOfInsurance']['value']['interpretedValue'] != "Collision Insurance":
    #     return {
    #         'isValid': False,
    #         'incorrectOption': True,
    #         'violatedSlot': 'TypeOfInsurance'
    #     }
    
    return {'isValid': True}

def valid_slot_insurance_upload(slots):

    if not slots['InsurerName'] or not slots['InsurerName'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'InsurerName'
        }
        
    if not slots['CardUpload'] or not slots['CardUpload'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'CardUpload'
        }
    
    return {'isValid': True}
    
def valid_slot_appointment_confirmation(slots):

    if not slots['AppointmentSelection'] or not slots['AppointmentSelection'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'AppointmentSelection'
        }
        
    if not slots['CardUpload'] or not slots['CardUpload'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'CardUpload'
        }
    
    return {'isValid': True}

def valid_slot_follow_up(slots):
    if not slots['AnyMore'] or not slots['AnyMore'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'AnyMore'
        }
    
    if not "yes" in slots['AnyMore']['value']['interpretedValue'].lower():
        return {
            'isValid': False,
            'incorrectOption': True,
            'violatedSlot': 'AnyMore'
        }
    
    if not slots['AutoPayment'] or not slots['AutoPayment'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'AutoPayment'
        }
    
    if slots['AutoPayment']['value']['interpretedValue'] != 'Change Payment Method':
        return {
            'isValid': False,
            'incorrectOption': True,
            'violatedSlot': 'AutoPayment'
        }
    
    if not slots['PaymentMethod'] or not slots['PaymentMethod'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'PaymentMethod'
        }
    
    if slots['PaymentMethod']['value']['interpretedValue'] != 'New Card':
        return {
            'isValid': False,
            'incorrectOption': True,
            'violatedSlot': 'PaymentMethod'
        }
    
    if not slots['CardDetails'] or not slots['CardDetails'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'CardDetails'
        }
    
    if not slots['AnythingElse'] or not slots['AnythingElse'].get('value', {}).get('interpretedValue', ''):
        return {
            'isValid': False,
            'incorrectOption': False,
            'violatedSlot': 'AnythingElse'
        }
    
    if slots['AnythingElse']['value']['interpretedValue'] != 'No':
        return {
            'isValid': False,
            'incorrectOption': True,
            'violatedSlot': 'AnythingElse'
        }
    
    return {'isValid': True}

def lambda_handler(event, context):
    logger = logging.getLogger()
    logger.setLevel(logging.INFO)
    logger.info(event)
    hander = Handler(event, logger)
    response = hander.process()
    print(response)
    return response
    
class Handler:
    def __init__(self, event, logger):
        with open('config.json', encoding="utf-8") as configin:
            self.config = json.load(configin)
        self.event = event
        self.session = event['sessionState']
        self.invocation_source = event['invocationSource']
        self.current_intent =  self.session['intent']['name']
        self.slots = self.session['intent']['slots']
        self.intent_type = "Close"
        self.state = "Fulfilled"
        self.messages = []
        self.contexts = []
        self.slot_to_elicit = None
        self.session_attributes = None
        self.request_attributes = None
        self.context_attributes = None
        self.logger = logger
        print("Slot Info: ", self.slots)
        
    def add_message(self, content, content_type=None):
        ctype, cont = "PlainText", "Unsupported Content"
        if not content_type or content_type == "PlainText":
            ctype = "PlainText"
            cont = content
        elif content_type == "CustomPayload":
            ctype, cont = content_type, json.dumps(content)
        self.messages.append({"contentType": ctype, "content": cont})
        
    def welcome(self):
        valid_all_slot = valid_slot_welcome(self.slots)

        if self.invocation_source == 'DialogCodeHook':
            if not valid_all_slot['isValid']:
                self.intent_type = "Close"
                self.slot_to_elicit = valid_all_slot['violatedSlot']
                self.state = "InProgress"

                self.add_message(self.config['welcome']['slot_prompt'][valid_all_slot['violatedSlot']], "CustomPayload")
            else:
                self.intent_type = "Delegate"
                self.state = "ReadyForFulfillment"
    
    def schedule_appointment(self):
        valid_all_slot = valid_slot_schedule_appointment(self.slots)

        if self.invocation_source == 'DialogCodeHook':
            if not valid_all_slot['isValid']:
                self.intent_type = "ElicitSlot"
                self.slot_to_elicit = valid_all_slot['violatedSlot']
                self.state = "InProgress"

                if valid_all_slot['incorrectOption']:
                    self.add_message(self.config['schedule_appointment']['incorrect_option'][valid_all_slot['violatedSlot']], "CustomPayload")
                # elif valid_all_slot['fallback']:
                #     self.add_message(self.config['schedule_appointment']['slot_prompt'][valid_all_slot['violatedSlot']], "CustomPayload")
                else:
                    self.add_message(self.config['schedule_appointment']['slot_prompt'][valid_all_slot['violatedSlot']], "CustomPayload")
            else:
                self.intent_type = "Delegate"
                self.state = "ReadyForFulfillment"
                
    def insurance_upload(self):
        valid_all_slot = valid_slot_insurance_upload(self.slots)

        if self.invocation_source == 'DialogCodeHook':
            if not valid_all_slot['isValid']:
                self.intent_type = "ElicitSlot"
                self.slot_to_elicit = valid_all_slot['violatedSlot']
                self.state = "InProgress"

                if valid_all_slot['incorrectOption']:
                    self.add_message(self.config['insurance_upload']['incorrect_option'][valid_all_slot['violatedSlot']], "CustomPayload")
                else:
                    self.add_message(self.config['insurance_upload']['slot_prompt'][valid_all_slot['violatedSlot']], "CustomPayload")
            else:
                self.intent_type = "Delegate"
                self.state = "ReadyForFulfillment"
                
    def appointment_confirmation(self):
        valid_all_slot = valid_slot_appointment_confirmation(self.slots)

        if self.invocation_source == 'DialogCodeHook':
            if not valid_all_slot['isValid']:
                self.intent_type = "ElicitSlot"
                self.slot_to_elicit = valid_all_slot['violatedSlot']
                self.state = "InProgress"

                if valid_all_slot['incorrectOption']:
                    self.add_message(self.config['appointment_confirmation']['incorrect_option'][valid_all_slot['violatedSlot']], "CustomPayload")
                else:
                    self.add_message(self.config['appointment_confirmation']['slot_prompt'][valid_all_slot['violatedSlot']], "CustomPayload")
            else:
                self.intent_type = "Delegate"
                self.state = "ReadyForFulfillment"
    
    def follow_up(self):
        valid_all_slot = valid_slot_follow_up(self.slots)

        if self.invocation_source == 'DialogCodeHook':
            if not valid_all_slot['isValid']:
                self.intent_type = "ElicitSlot"
                self.slot_to_elicit = valid_all_slot['violatedSlot']
                self.state = "InProgress"

                if valid_all_slot['incorrectOption']:
                    self.add_message(self.config['follow_up']['incorrect_option'][valid_all_slot['violatedSlot']], "CustomPayload")
                else:
                    self.add_message(self.config['follow_up']['slot_prompt'][valid_all_slot['violatedSlot']], "CustomPayload")
            
            else:
                self.intent_type = "Delegate"
                self.state = "ReadyForFulfillment"
    
    def process(self):
        intent = self.current_intent
        if intent == 'Welcome':
            self.welcome()
        if intent == 'ScheduleAppointment':
            self.schedule_appointment()
        if intent == 'InsuranceUpload':
            self.insurance_upload()
        if intent == 'AppointmentConfirmation':
            self.appointment_confirmation()
        if intent == 'FollowUp':
            self.follow_up()
            
        return self.generate_response()
        
    def generate_response(self):
        response = {
            "sessionState": {
                "sessionAttributes": self.session.get("sessionAttributes", {}),
                "activeContexts": self.contexts,
                "dialogAction": {
                    "slotToElicit": self.slot_to_elicit,
                    "type": self.intent_type
                },
            },
            "messages": self.messages,
            "requestAttributes": self.session.get("requestAttributes", {})
        }
        
        if self.intent_type not in ("ElicitIntent",):
            response["sessionState"]["intent"] = {
                "name": self.current_intent,
                "state": "InProgress",
                "slots": self.slots
            }
            
        self.logger.info(response)
        return response




        {
    "welcome": {
        "slot_prompt": {
            "WelcomeMessage": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Welcome to our Imaging Self-Service Centre. How may I assist you today? Please click one of the options below, to begin."
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "text",
                    "align": "Vertical",
                    "buttons": [
                        {
                            "ButtonName": "Schedule an Imaging appointment ",
                            "ButtonValue": "Schedule an Imaging appointment "
                        },
                        {
                            "ButtonName": "Help on an existing Imaging appointment",
                            "ButtonValue": "Help on an existing Imaging appointment"
                        }
                    ]
                }
            }
        }
    },
   "schedule_appointment": {
        "slot_prompt": {
            "CustomerName": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "I'd be happy to assist you with your appointment.To get started, could you please provide your first and last name?"
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "text",
                    "align": "",
                    "buttons": []
                }
            },
            "DateOfBirth": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Thank you, {CustomerName}. Could you also provide your date of birth (i.e., MM/DD/YYYY)"
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "date",
                    "align": "",
                    "buttons": []
                }
            },
            "PostalCode": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Thank you. Lastly, could you please provide your zip-code?"
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "text",
                    "align": "",
                    "buttons": []
                }
            },
            "CenterOption": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Based on the zip-code: {PostalCode}, {CustomerName}, I've found the following centers near your location. Please choose one of the options below to proceed."
                    }
                ],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "Horizontal",
                    "buttons": [
                        {
                            "ButtonName": "Monroe",
                            "ButtonValue": "Monroe"
                        },
                        {
                            "ButtonName": "New Hanover",
                            "ButtonValue": "New Hanover"
                        },
                        {
                            "ButtonName": "Charlotte",
                            "ButtonValue": "Charlotte"
                        }
                    ]
                }
            },
            "CenterConfirmation": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Are you satisfied with this location, or would you prefer choosing a different one?"
                    }
                ],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "Horizontal",
                    "buttons": [
                        {
                            "ButtonName": "Yes",
                            "ButtonValue": "Yes"
                        },
                        {
                            "ButtonName": "Change Location",
                            "ButtonValue": "Change Location"
                        }
                    ]
                }
            },
            "InsuranceOption": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Thank you, {CustomerName}. Will you be using insurance for this appointment?"
                    }
                ],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "Horizontal",
                    "buttons": [
                        {
                            "ButtonName": "Yes",
                            "ButtonValue": "Yes"
                        },
                        {
                            "ButtonName": "No",
                            "ButtonValue": "No"
                        }
                    ]
                }
            }
        },
        "incorrect_option": {
            "CenterOption": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Please choose one of the location below to proceed."
                    }
                ],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "Horizontal",
                    "buttons": [
                        {
                            "ButtonName": "Monroe",
                            "ButtonValue": "Monroe"
                        },
                        {
                            "ButtonName": "New Hanover",
                            "ButtonValue": "New Hanover"
                        },
                        {
                            "ButtonName": "Charlotte",
                            "ButtonValue": "Charlotte"
                        }
                    ]
                }
            }
        }
    },
    "insurance_upload": {
        "slot_prompt": {
            "InsurerName": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Can you please enter the name of your insurance provider below?"
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "text",
                    "align": "",
                    "buttons": []
                }
            },
            "CardUpload": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Thank you. Kindly upload a photo of your insurance card."
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "upload",
                    "align": "",
                    "buttons": []
                }
            }
        }
    },
    "appointment_confirmation": {
        "slot_prompt": {
            "AppointmentSelection": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Perfect! Let's go ahead and find a suitable appointment time for you."
                    },
                    {
                        "text": "Please choose your preferred date and time for your appointment."
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "date",
                    "align": "",
                    "buttons": []
                }
            },
            "SlotConfirmation": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "I see you've selected {AppointmentSelection}."
                    },
                    {
                        "text": "Would you like to proceed with this time, or would you prefer to choose a different date and time?"
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "upload",
                    "align": "",
                    "buttons": [
                        {
                            "ButtonName": "Yes",
                            "ButtonValue": "Yes"
                        },
                        {
                            "ButtonName": "Select Other Date and Time",
                            "ButtonValue": "Select Other Date and Time"
                        }
                    ]
                }
            }
        }
    },
    "follow_up": {
        "slot_prompt": {
            "AnyMore": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Thank you for providing all the necessary information. We have successfully received your claim and will assign an adjuster to your case within 2 business days. You will receive an email with their contact information and further instructions."
                    },
                    {
                        "text": "Do you have any other questions or concerns?"
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "text",
                    "align": "",
                    "buttons": []
                }
            },
            "AutoPayment": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Certainly! Would you like to change the date of auto-payment or payment method"
                    }
                ],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "Horizontal",
                    "buttons": [
                        {
                            "ButtonName": "Change Date",
                            "ButtonValue": "Change Date"
                        },
                        {
                            "ButtonName": "Change Payment Method",
                            "ButtonValue": "Change Payment Method"
                        }
                    ]
                }
            },
            "PaymentMethod": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Do you like to change the payment method details"
                    }
                ],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "Horizontal",
                    "buttons": [
                        {
                            "ButtonName": "Edit Existing Card Details",
                            "ButtonValue": "Edit Existing Card Details"
                        },
                        {
                            "ButtonName": "New Card",
                            "ButtonValue": "New Card"
                        }
                    ]
                }
            },
            "CardDetails": {
                "type": "creditCard",
                "messages": [],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "",
                    "buttons": []
                }
            },
            "AnythingElse": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "Your new payment information has been successfully changed. Is there anything more I can assist you with?"
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "text",
                    "align": "",
                    "buttons": []
                }
            }
        },
        "incorrect_option": {
            "AnyMore": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "For the purposes of this demo, please enter <b>Yes, I would like to change my auto payment details.</b>"
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "text",
                    "align": "",
                    "buttons": []
                }
            },
            "AutoPayment": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "For the purposes of this demo, please select <b>Change Payment Method</b>"
                    }
                ],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "Horizontal",
                    "buttons": [
                        {
                            "ButtonName": "Change Date",
                            "ButtonValue": "Change Date"
                        },
                        {
                            "ButtonName": "Change Payment Method",
                            "ButtonValue": "Change Payment Method"
                        }
                    ]
                }
            },
            "PaymentMethod": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "For the purposes of this demo, please select <b>New Card</b>"
                    }
                ],
                "input": {
                    "inputText": false,
                    "masked": false,
                    "inputType": false,
                    "align": "Horizontal",
                    "buttons": [
                        {
                            "ButtonName": "Edit Existing Card Details",
                            "ButtonValue": "Edit Existing Card Details"
                        },
                        {
                            "ButtonName": "New Card",
                            "ButtonValue": "New Card"
                        }
                    ]
                }
            },
            "AnythingElse": {
                "type": "confirmation",
                "messages": [
                    {
                        "text": "For the purposes of the demo, please enter <b>No</b>."
                    }
                ],
                "input": {
                    "inputText": true,
                    "masked": false,
                    "inputType": "text",
                    "align": "",
                    "buttons": []
                }
            }
        }
    }
}
