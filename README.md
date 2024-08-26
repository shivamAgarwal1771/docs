    toggleLike() {
        this.donelike1 = !this.donelike1;
        this.doneUnlike1 = true;  // Reset the dislike state when like is toggled
        const event = new ShowToastEvent({
            title: this.donelike1 ? 'Liked the Assist.' : 'Disliked the Assist.',
            message: 'Thanks for providing feedback',
            variant: this.donelike1 ? 'warning' : 'success',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }

    toggleDislike() {
        this.doneUnlike1 = !this.doneUnlike1;
        this.donelike1 = true;  // Reset the like state when dislike is toggled
        const event = new ShowToastEvent({
            title: this.doneUnlike1 ? 'Disliked the Assist.' : 'Liked the Assist.',
            message: 'Thanks for providing feedback',
            variant: this.doneUnlike1 ? 'warning' : 'error',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }

<template>
    <template lwc:if={showAssist}>
        <!-- ########## HEADER AI-ASSISTANCE  ########## -->
        <lightning-card>
            <div>
                <span> <img src={svgUrl} class="roboIconHeaderCss" /> </span> &nbsp;
                <span class="headerCSS"> AI Assistant </span>
            </div>
        </lightning-card>

        <!-- ########## AI-GUIDANCE  ########## -->
        <template lwc:if={guidanceTitle}>
            <lightning-card>
                <div class="secondContainer">
                    <h1 class="heading">{guidanceTitle}</h1>
                    <div class="sign">
                        <div class="like_dislike_iconCss">
                            <div class="likeCss">
                                <img src={likeIcon} alt="Like Icon" onclick={toggleLike} />
                            </div>
                            <div class="dislikeCss">
                                <img src={dislikeIcon} alt="Dislike Icon" onclick={toggleDislike} />
                            </div>
                        </div>
                    </div>
                    <template for:each={guidanceData} for:item="item" for:index="index">
                        <p class="paragraph" key={item.key}>{item.value}</p>
                    </template>
                </div>
            </lightning-card>
        </template>

        <!-- ########## SPEECH SUGGESTION  ########## -->
        <template lwc:if={speach_suggestion}>

            <lightning-card>
                <div class="secondContainer">
                    <h1 class="heading">
                        Speech Suggestions
                    </h1>
                    <div class="sign">
                        <div class="like_dislike_iconCss">
                            <div class="likeCss">
                                <img src={likeIcon1} alt="Like Icon" onclick={toggleLike1} />
                            </div>
                            <div class="dislikeCss">
                                <img src={dislikeIcon1} alt="Dislike Icon" onclick={toggleDislike1} />
                            </div>
                        </div>
                    </div>

                    <div>
                        <div class="dottedBoxCss">
                            <div class="dottedboxTEXT_Css">
                                <h1>{speach_suggestion}</h1>
                            </div>
                        </div>
                    </div>
                </div>
            </lightning-card>

        </template>

        <!-- ########## KNOWLEDGE ARTICLES  ########## -->
        <template lwc:if={knowledgeArticlesName}>
            <lightning-card>
                <div class="secondContainer">
                    <h1 class="heading">
                        {knowledgeArticlesName}
                    </h1>

                    <div class="sign">
                        <div class="like_dislike_iconCss">
                            <div class="likeCss">
                                <img src={likeIcon2} alt="Like Icon" onclick={toggleLike2} />
                            </div>
                            <div class="dislikeCss">
                                <img src={dislikeIcon2} alt="Dislike Icon" onclick={toggleDislike2} />
                            </div>
                        </div>
                    </div>

                    <div class="slds-box boxCss">
                        <p>{knowledgeArticlesTitle}</p>

                        <lightning-button variant="base" label={buttonLabel} onclick={handleToggle}></lightning-button>

                        <div if:false={isDivVisible} class="slds-m-top_medium showMore">
                            <template for:each={knowledgeArticlesValue} for:item="item" for:index="index">
                                <p class="paragraph" key={item.key}>
                                    {item.value}
                                </p>
                            </template>
                        </div>
                    </div>
                </div>
            </lightning-card>
        </template>

        <!-- ########## KNOWLEDGE ARTICLES BODY ########## -->
        <template if:true={isModalOpen}>
            <!--<c-awc_knowledge_article article-url={articleUrl} onclose={handleCloseModal}></c-awc_knowledge_article>-->
            <template for:each={knowledgeArticlesValue} for:item="KnowledgeArticle" for:index="ka">
                <div key={KnowledgeArticle}>
                    {KnowledgeArticle}
                </div>
            </template>
        </template>

        <!-- ########## WORKFLOW ########## -->
        <template if:true={botId}>
            <lightning-card>
                <!--#### HEADER ####-->
                <div class="secondContainer">
                    <h1 class="heading">
                        Action Workflow
                    </h1>
                </div>
                <br>

                <!--Run WF Functionality-->
                <template if:true={isJsonShowing}>
                    <template for:each={jsonData.data.getAutomationBotResponse} for:item="workflow" for:index="index">
                        <div key={intentDetected}>
                            <div>
                                <h4 style="margin-left: 25px;"><b>Intent Detected</b>&nbsp;:&nbsp;
                                    <span style="color:#A32EFF;">{intentDetected}</span>
                                </h4>
                                <span> <img src={edit_icon} class="roboIconCss" /> </span> &nbsp;
                            </div>
                            <br>
                            <h4 style="margin-left: 25px;"><b>Proposed Workflow</b></h4>
                            <br>
                            <div
                                style="display: flex; width: 89%; border: 1px solid lightgray; margin-left: 35px; padding: 10px; border-radius: 5px;">
                                <div class="runSectionCss"><b>Start Workflow</b></div>
                                <button class="runbuttonCss" data-index={index} onclick={handleRunClick}>Run</button>
                            </div><br>
                        </div>
                    </template>
                </template>

                <!--Completed Workflow-->
                <template if:true={completedWorkflow}>
                    <h4 style="margin-left: 25px;"><b>Completed Workflow</b></h4><br>
                    <div
                        style="display: flex; width: 89%; border: 1px solid lightgray; margin-left: 35px; padding: 10px; border-radius: 5px;">
                        <div style="width:70%;"><b>{intentDetected}</b></div>
                        <button class="seeDetailsCss" onclick={seeCompletedWorkflow}>See Details</button>
                    </div>
                </template>

                <lightning-spinner if:true={isLoading} alternative-text="Loading"></lightning-spinner>
                <!--Intent Detected Workflow-->
                <template if:false={isLoading}>
                    <template if:true={ifClickOnParticularWorkflow}>
                        <div class="slds-p-around_medium">
                            <div if:true={intentDetected}>
                                <h4 style="margin-left: 25px;"><b>Intent Detected</b>&nbsp;:&nbsp;
                                    <span style="color:#A32EFF;">{intentDetected}</span>
                                </h4><br>

                                <h4 style="margin-left: 25px;"><b>Default Workflow: Initiator</b></h4>
                                <b style="margin-left: 25px;">Total Steps &nbsp; &nbsp; {totalSteps}</b>
                                <span> <img src={edit_icon} class="stopwfpencilIconCss" /> </span> &nbsp;
                            </div>
                            <button class="stopworkflow" onclick={handleStopWorkFlow}>Stop Workflow</button>
                            <br>

                            <div if:true={intentDetected}>

                                <!--Workflow Fields-->
                                <template for:each={steps} for:item="step">
                                    <div key={step.stepName}>
                                        <div class="slds-box" style="margin-top: 7px; border: 2px solid #f0ecec;">
                                            <lightning-input label={step.stepName} value={step.messageText}
                                                disabled={step.isEditable}></lightning-input>
                                        </div>
                                    </div>
                                </template>

                                <!--Mark workflow as cpmpleted Button-->
                                <template if:true={isDisabled}>
                                    <button class="wfCompletedButtonCss" data-index={index} onclick={handleSave}>Mark
                                        workflow as completed</button>
                                </template>
                                <template if:false={isDisabled}>
                                    <button class="wfCompletedButtonCss_disabled" data-index={index}
                                        onclick={handleSave} disabled={isDisabled}>Mark workflow as completed</button>
                                </template>

                            </div>
                        </div>
                    </template>
                    <template if:true={showRecentUpdatedActionWorkflow}>
                        <div class="slds-p-around_medium">
                            <b>Total Steps &nbsp;:&nbsp; {totalSteps}</b>
                            <button class="backButtonCss" data-index={index} onclick={handleBack}>Back</button>
                            <!--Workflow Fields-->
                            <template for:each={steps} for:item="step">
                                <div key={step.stepName}>
                                    <div class="slds-box" style="margin-top: 7px; border: 2px solid #f0ecec;">
                                        <lightning-input label={step.stepName} value={step.messageText}
                                            disabled="true"></lightning-input>
                                    </div>
                                </div>
                            </template>
                            <!--Date Fields-->
                            <template if:true={newmovementDate}>
                                <lightning-input type="date" label="Date of Movement" value={newmovementDate}
                                    disabled></lightning-input>
                            </template>
                        </div>
                    </template>
                </template>
            </lightning-card>
        </template>
    </template>
</template>
