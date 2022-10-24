@startuml
actor "patient" as patient
participant "<website>\nQueueTy" as app
actor "front\ndesk" as frontdesk
participant "<System>\nprovider" as provider
participant "<SaaS>\nQueueTy" as qt


== patient registration via QTs ==
patient -> app : discover providers and queuing time
patient -> patient: go to provider office
patient -> frontdesk: register for service\nprovide phone number for text confirmation
frontdesk -> provider: add patient to queue
provider -> qt: send new patient registered
qt --> patient: send confirmation text with\nlink to wait list


== provider interaction with QT ==
provider -> provider : process patient sequentially from queue
loop for each patient processed
  provider -> qt: update queue status
end

== patient waiting to be served by provider ==
patient -> app: query my queue status at provider site
app -> qt: query status for patient
app --> patient: latest queue status

== alert patient provider is now available ==
provider -> qt: now ready to serve patient
qt -> patient: push text message alert
patient -> frontdesk: check in




@enduml