<script>
  import { Accordion, AccordionItem } from "sveltestrap";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
</script>

<div
  style="background: linear-gradient(to bottom right,white,whitesmoke);height: 100vh;"
>
  <Navbar color="text-black" btnTheme=" btn-dark" />
  <div class="container mt-5">
    <div class="text-center">
      <h1>Do you have a question?</h1>
      <h4>Here are some of the Frequently asked questions</h4>
      <span
        >No need to queue for our customer service agents! You can manage,
        change, or cancel your booking online.</span
      >
    </div>
    <br />
    <div class="row mt-5">
      <div class="col-md-6 mt-5">
        <h4>Booking Mechanism</h4>
        <Accordion>
          <AccordionItem
            header="How does booking mechanism works in Digital Marketplace for Tourism aka DMT ?"
          >
            You as a buyer, request the booking directly from the service
            provider. Once the seller confirms your booking, you have your
            reservation. Now, your booking is confirmed. This will also give you
            an ease of mind to organize your future plans because we value your
            peace of mind.
          </AccordionItem>
          <AccordionItem
            header="How will I know whether my booking is confirmed ?"
          >
            You will receive a confirmation email from the seller regarding your
            calender dates addressing your special requirement (if any) for
            those specific dates.
          </AccordionItem>
          <AccordionItem
            header="Will my Payment be deducted after I send my booking request ?"
          >
            No, not yet. By this time, we will only save your payment details.
            As soon as there is confirmation from the seller, your allocated
            amount will be on hold. For your assurance, after you check in for
            the services, the amount is only deducted at this time and
            transferred to your seller.
          </AccordionItem>
        </Accordion>
        <br />
        <h4>Cancellation and getting refund</h4>
        <Accordion>
          <AccordionItem header="What if I want to cancel my booking ?">
            Your booking can easily be cancelled without any charges if your
            booking request has not been approved by the seller yet. However,
            lets say - your booking request has already been accepted by the
            seller. Now, in that scenario, if you have a free cancellation
            booking for that property or experience, you won't pay a
            cancellation fee. If your booking is no longer free to cancel or is
            non-refundable, you may incur a cancellation fee. As mentioned
            above, any cancellation fees for a booking is determined by the
            property or the experience provider.
          </AccordionItem>
          <AccordionItem header="When will I receive my refund amount ?">
            If your booking request had not been approved by the seller and you
            canceled the request, your fund had never been deducted in the first
            place. However, if your booking request was accepted by the seller
            and you cancelled the booking, the allocated amount after
            cancellation charge (if any) should be in your bank in no time as
            your amount was only in hold. You can also track the progress of
            your refund in your [Booking Status Page].
          </AccordionItem>
        </Accordion>
        <br />
        <h4>Change or extend the dates</h4>
        <Accordion>
          <AccordionItem
            header="What if I want to change my booking date or extend my stay ?"
          >
            You can easily connect to the seller via our direct message services
            and change or extend the dates if the new date is available.
          </AccordionItem>
          <AccordionItem header="What if I have some special request ?">
            You can always respond to the seller through our connected messaging
            service.
          </AccordionItem>
          <AccordionItem
            header="Is Subscription Plan available for all types of users ?"
          >
            No. it’s only available for sellers right now to enhance their
            business operations as most of them are local providers. This will
            also help them to systematize their business operation and boost the
            local economy
          </AccordionItem>
        </Accordion>
      </div>
      <div class="col-md-6 mt-5">
        <img
          src="/assets/static/images/global/faqs.jpg"
          alt=""
          height="100%"
          width="100%"
          style="border-radius: 5pt;object-fit: cover;"
        />
      </div>
    </div>
  </div>
</div>
