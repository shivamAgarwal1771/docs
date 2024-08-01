<template>
  <lightning-card
    icon-name="utility:emoji"
    title="Sentiment Trend"
    class="Purple-utility-icon" >
    <!-- <div class="slds-box"> -->
      <!-- <div class="slds-grid slds-wrap slds-grid--pull-padded"> -->
        <div
          if:true={isChartJsInitialized}
          class="slds-col--padded slds-size--1-of-1"
        >
          <!--<canvas class="linechart" ></canvas>-->
          <div class="chart-container">
            <canvas class="linechart" width="400" height="80"></canvas>
          </div>
        </div>
      <!-- </div> -->
    <!-- </div> -->
  </lightning-card>
</template>


.emoji {
  color: #888; /* Apply red color to emoji */
}

.chart-container {
  width: 100%;
}

.sentiment-container {
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}




<!--
@description       : 
@author            : Rajat Rohila
@group             : 
@last modified on  : 05-27-2024
-->
<template>
    <div class="message-container" if:false={isCallEnded}>
            <!-- HEADER WITH CALL START TIME -->
            <!--<div class="header">
                <lightning-card title='Transcript' icon-name="custom:custom112" size="small" class='gray-utility-icon'>
                    <span if:true={showTime} slot="actions" class="timeHeader">Call Started: &nbsp; <lightning-formatted-date-time value={formattedTime} hour="2-digit" minute="2-digit" hour12={ampm}></lightning-formatted-date-time></span>
                </lightning-card>
            </div>-->
            <!-- <div class="call-insight_header header-shadow p-t-b-8px">
                <div class="call-insight_title">
                        <img src={icon1} class="roboIconCss"/>
                      <p>Transcript</p>
                    </div>
                <div class="time-tag">Call started: <lightning-formatted-date-time value={formattedTime} hour="2-digit" minute="2-digit" hour12={ampm}></lightning-formatted-date-time> IST</div>
            </div> -->
            <lightning-card>
                <div class="transcript-header">
                    <div class="transcript-tag">
                        <img src={icon1} class="roboIconCss"/>
                        <span class="headerCss"> Transcript </span>
                    </div>
                    <div if:true={showTime} slot="actions" class="time-tag">Call Started: <lightning-formatted-date-time value={formattedTime} hour="2-digit" minute="2-digit" hour12={ampm}></lightning-formatted-date-time></div>
                </div>
            </lightning-card>

            <!-- BODY WITH TRANSCRIPT -->

            <lightning-card>
             <div class="card-content">
                <ul class="custom-card">
                    <template for:each={messages} for:item="message">
                        <li key={message.id} >
                           <div class="container" style="padding: 16px 16px 0px 16px;">
                                <div class="header">
                                    <div class="message-info">

                                        <!--AVTAR ICON-->
                                            <!--<lightning-icon icon-name={message.icon} size="small" class="img-circle"></lightning-icon>-->
                                        <img src={message.icon}  class="img-circle"/>

                                        <!--TRANSCRIPT-->
                                        <span class={message.class}>
                                            {message.speaker}
                                        </span> &nbsp;&nbsp;

                                        <!--SENTIMENT EMOJI-->

                                        <template if:true={message.isCustomer}>
                                            <!--<lightning-icon icon-name="utility:emoji" size="x-small" alternative-text="Check Icon" style="color: #49A54C;"></lightning-icon>-->
                                            <img src={emojee}  class="img-circle"/>
                                        </template>

                                    </div>

                                    <!--TRANSCRIPT TIME-->
                                    <div>
                                        <p class="timeText"><lightning-formatted-date-time value={message.time} hour="2-digit" minute="2-digit"></lightning-formatted-date-time></p>
                                    </div>
                                    
                                </div>
                                <div class="message-text">
                                    <span>{message.text}</span>
                                </div>
                           </div>
                        </li>
                    </template>
                </ul>
            </div>
        </lightning-card>
    </div>
</template>


.message-container {
    overflow-y: auto;
    scroll-behavior: smooth;
    margin-bottom: 10px;
}
  
.custom-card {
    -ms-overflow-style: none; 
    overflow-y: scroll; 
    height: 35vh;
}

.card-content {
    max-height: 100%; /* Ensures the content doesn't exceed the height of the card */
    padding: 0px 0px 15px 0px;
}
  
.agent-message {   
    font-family: math;
    font-size: 14px;
    font-family: sans-serif;
    color: #2A2A2A;
    font-weight: 700;
    margin-left: 0.8rem;
}
  
.Customer-message {
    color: #4409A1;
    font-family: math;
    font-weight: 700;
    font-size: 14px;
    font-family: sans-serif;
    margin-left: 0.8rem;
}
  
.icon_container{
    margin-right: 2px; 
    padding: 7px; 
    border-radius: 50%; 
    background: #F8F8F8; 
    height: 6vh; 
    width: 6vh;
}

.circle{
    margin-top: 10px;
    width: 44px;
    height: 38px;
    background-color: green;
    border-radius: 50%;
    color: green;
}
.message-info {
    display: flex;
    justify-content: start;
    align-items: center;
    width: 9.5rem;
    height: 8vh;
    margin-top: -13px;
}
 
.timeHeader{
    color: #bbb3b3;
    font-size: 14px;
    font-weight: 500;
    float: right;
    padding-right: 5%;
}
  
.timeText{
    color: #A09FA3; 
    font-size: 14px;
    font-weight: 600;
}


.message-text {
    font-size: 14px;
    color: #2A2A2A;
    margin-left: 2.5rem;
    margin-top: -2vh;
}

.header{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

hr {
    display: block;
    margin: 0 px;
    margin: auto;
    border-top: 5px solid lightgray;
    height: 1px;
    clear: both;
}
  
.caller_CSS {
    color: #4409A1;
    font-family: math;
    font-weight: 600;
    font-size: medium;
    font-family: sans-serif;
    margin-left: 0.8rem;
}

.gray-utility-icon {
    --sds-c-icon-color-foreground-default: #B8B8BA;
    width: 100%;
}

.roboIconCss{
    padding-left: 3%;
}

.headerCss{
    line-height: initial;
    font-weight: 700;
    font-size: 1rem;
}

.call-insight_header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: #fff;
    /* box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); */
    border-radius: 10px 10px 0 0;
    margin-bottom: 8px;
    padding: 8px 1rem 6px;
    border: 1px solid #ebf1fb;
    height: 52px;
}

.header-shadow {
    /* box-shadow: 0px 4px 4px 0px #00000026 !important; */
}

.p-t-b-8px {
    padding: 8px 1rem;
}

.call-insight_title {
    display: flex;
    align-items: center;
    gap: 10px;
}
.call-insight_title p {
    font-weight: 700;
    font-size: 1rem;
}

.time-tag {
    color: #a09fa3;
    font-size: 1em;
    font-weight: 600;
}

.upperContainer{
    margin-top: -2vh;
}

.container1{
    display: flex;
    flex-direction: row;
    align-items: center;
    background-color: #ffffff;
    /* width: 450px; */
    justify-content: space-between;
    height: 52px;
    padding: 0px 16px 0px 16px;
    border-radius: 6px 6px 0px 0px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    margin-bottom: 5px;
    margin-top: 5px;
}
.container1_2{ 
    background-color: #ffffff; 
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    padding: 20px 2px 20px 2px;
    /* width: 450px; */
     justify-content: space-between;
    border-radius: 0px 0px 6px 6px;
    margin-bottom: 5px; 
}

.transcript-header {
    display: flex;
    gap: 10px;
    padding: 0px 13px;
    justify-content: space-between;
    text-align: center;
}

.transcript-tag{
    display: flex;
    gap: 5px;
}


<template>
  <template if:false={isSubtab}>
    <lightning-card>
      <div class="audit-header-container">
        <div class="audit-header">
          <span> <img src={autoAudit} alt="Burger_Menu" /> </span>
          <span class="header-css"> Auto-audit </span>
        </div>
        <div class="buger-menu-container">
          <span slot="actions" class="time-tag">{percentage}%</span>
          <img
            src={menu}
            alt="Burger_Menu"
            onclick={openAutoAuditPopup}
            class="header-icon"
          />
          <!-- onclick={onOpenAccountClick} -->
          <img
            src={accordion}
            alt="Accordion"
            onclick={hideAutoAudit}
            class={auditOpenClass}
          />
        </div>
      </div>
    </lightning-card>
    <lightning-card if:true={showAudit}>
      <ul>
        <template
          for:each={processQuestion}
          for:item="process"
          for:index="index"
        >
          <li key={process.Id} style="margin-bottom: 20px">
            <div class="container liContainer">
              <div class="processQuestionContainer">{process.question}</div>
              <div
                style="
                  width: 19px;
                  height: 22px;
                  margin-right: 13px;
                  display: flex;
                  flex-direction: row;
                  justify-content: center;
                  align-items: center;
                "
              >
                <img
                  src={process.icon}
                  alt="Minus"
                  style="margin-right: 10px"
                />

                <!-- <lightning-icon icon-name="utility:chevrondown" onclick={toggleRotation} class={iconClass} data-index={index} aria-controls={process.Id}></lightning-icon> -->
                <img
                  src={accordion}
                  alt="Accordion"
                  onclick={toggleRotation}
                  class={iconClass}
                  data-index={index}
                  aria-controls={process.Id}
                />
              </div>
            </div>
            <div
              style="
                margin-left: 20px;
                margin-top: 10px;
                color: #a09fa3;
                font-weight: 700;
                line-height: 16.8px;
                font-size: 14px;
              "
            >
              <template if:true={process.rotation}>
                <template
                  for:each={process.counterQuestions}
                  for:item="counterQuestion"
                >
                  <div key={counterQuestion.Id} style="margin-bottom: 5px">
                    {counterQuestion.questionText}
                    <div
                      style="
                        display: flex;
                        flex-direction: row;
                        align-items: center;
                        width: 100%;
                        margin-top: 10px;
                      "
                    >
                      <template
                        for:each={counterQuestion.options}
                        for:item="option"
                      >
                        <div
                          key={option.value}
                          style="
                            display: flex;
                            align-items: center;
                            margin-right: 10px;
                          "
                        >
                          <input
                            type="radio"
                            name={counterQuestion.questionText}
                            value={option.value}
                            checked={option.checked}
                            disabled={option.disabled}
                            onchange={handleOptionChange}
                          />
                          <label style="margin-left: 5px">{option.label}</label>
                        </div>
                      </template>
                    </div>
                  </div>
                </template>
              </template>
            </div>
          </li>
        </template>
      </ul>
    </lightning-card>
  </template>
  <template if:true={isSubtab}>
    <div>
      <div class="container1 upperContainer">
        <div class="container1_1">
          <img
            src={autoAudit}
            alt="Burger_Menu"
            style="height: 24px; width: 24px; margin-right: 2vh"
          />
          <div
            style="
              font-size: 18px;
              line-height: 24px;
              color: #2a2a2a;
              font-weight: 600;
            "
          >
            Auto-audit
          </div>
        </div>
      </div>
      <div class="container2_234">
        <div style="display: flex; width: 100%">
          <div style="width: 29%; margin-right: 1%" class="container2_2">
            <!-- <lightning-card> -->
            <div>
              <ul>
                <template
                  for:each={processQuestion}
                  for:item="process"
                  for:index="index"
                >
                  <li
                    key={process.Id}
                    style="margin-bottom: 20px; cursor: pointer"
                  >
                    <div
                      class="container liContainer"
                      onclick={questionHandler}
                      data-index={index}
                      aria-controls={process.Id}
                    >
                      <div class="processQuestionContainer">
                        {process.question}
                      </div>
                      <div
                        style="
                          width: 19px;
                          height: 22px;
                          margin-right: 13px;
                          display: flex;
                          flex-direction: row;
                          justify-content: center;
                          align-items: center;
                        "
                      >
                        <img
                          src={process.icon}
                          alt="Icon"
                          style="margin-right: 10px"
                        />
                      </div>
                    </div>
                  </li>
                </template>
              </ul>
            </div>
            <!-- </lightning-card> -->
          </div>
          <div style="width: 70%" class="container2_22">
            <!-- <lightning-card style="height: 300px;"> -->
            <template if:true={selectedCounterQuestions}>
              <template
                for:each={selectedCounterQuestions}
                for:item="counterQuestion"
              >
                <div
                  key={counterQuestion.Id}
                  style="margin-bottom: 5px; margin-left: 40px"
                >
                  {counterQuestion.questionText}
                  <div
                    style="
                      display: flex;
                      flex-direction: row;
                      align-items: center;
                      width: 100%;
                      margin-top: 10px;
                    "
                  >
                    <template
                      for:each={counterQuestion.options}
                      for:item="option"
                    >
                      <div
                        key={option.value}
                        style="
                          display: flex;
                          align-items: center;
                          margin-right: 10px;
                        "
                      >
                        <input
                          type="radio"
                          name={counterQuestion.id}
                          value={option.value}
                          checked={option.checked}
                          disabled={option.disabled}
                        />
                        <label style="margin-left: 5px">{option.label}</label>
                      </div>
                    </template>
                  </div>
                </div>
              </template>
            </template>
            <!-- </lightning-card> -->
          </div>
        </div>
      </div>
    </div>
  </template>
</template>


.container{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}
.upperContainer{
    margin-top: -2vh;
}
.container1_1{
    width: 131px;
    height: 24px;
    display: flex;
    /* gap: 10px; */
}
.liContainer{
    height: 25px;
    margin-left: 19px;
    margin-right: 20px;
}
.processQuestionContainer{
    font-weight: 600;
    font-size: 16px;
    line-height: 19.5px;
    color: #272727;
}
.rotated {
    transform: rotate(180deg);
}

.container1{
    display: flex;
    flex-direction: row;
    align-items: center;
    background-color: #ffffff;
    /* width: 450px; */
    justify-content: space-between;
    height: 52px;
    padding: 0px 16px 0px 16px;
    border-radius: 6px 6px 0px 0px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    margin-bottom: 5px;
    margin-top: 5px;
}
.container1_2{ 
    background-color: #ffffff; 
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    padding: 20px 2px 20px 2px;
    /* width: 450px; */
     justify-content: space-between;
    border-radius: 0px 0px 6px 6px;
    margin-bottom: 5px; 
 } 
.container2_2{ 
    background-color: #ffffff; 
     /* justify-content: space-between; */
    padding: 20px 2px 20px 2px;
    border-radius: 0px 0px 0px 6px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    margin-bottom: 5px; 
 }  
.container2_22{ 
    background-color: #ffffff; 
     /* justify-content: space-between; */
    padding: 20px 2px 20px 2px;
    border-radius: 0px 0px 6px 0px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    margin-bottom: 5px; 
 }  

.container1_22{
    background-color: #ffffff;
    /* width: 450px; */
    justify-content: space-between;
    padding: 20px 2px 20px 2px;
    border-radius: 0px 0px 6px 6px;
    /* box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); */
    margin-bottom: 5px;
}



.container1_11{
    display: flex;
    flex-direction: row;
    align-items: center;
    /* background-color: #ffffff; */
    justify-content: space-between;
    height: 16px;
    padding: 0px 16px 0px 16px;
    /* border-radius: 6px 6px 0px 0px; */
    /* box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); */
    margin-bottom: 5px;
    margin-top: 5px;
}

.time-tag {
    color: #a09fa3;
    font-size: 1em;
    font-weight: 600;
}

.header-css{
    line-height: initial;
    font-weight: 700;
    font-size: 1rem;
}

.audit-header {
    padding: 0px 13px;
    display: flex;
    justify-content: flex-start;
    text-align: center;
    gap: 4px;
}

.audit-header-container{
    display: flex;
    justify-content: space-between;
    padding-right: 10px;
}

.buger-menu-container{
    display: flex;
    gap: 5px;
}

.header-icon{
    cursor: pointer;
    height: 20px;
    width: 20px;
}

.rotated-icon {
    cursor: pointer;
    transform: rotate(180deg);
    height: 20px;
    width: 20px;
}
