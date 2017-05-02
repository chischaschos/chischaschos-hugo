+++
date = "2016-09-27T11:11:11-07:00"
title = "dependency inversion and rspec"
draft = true

+++

Let's say that I have a `BookingService`, it takes a `Document` and *books*
it for further processing.

The object could be called this way
```
document = Document.new
booking_service = BookingService.new(document)
booking_service.book!
```

This `BookingService` needs to backup the document, FTP it. Once this
document is posted in the FTP folder then another program would read it
and process it and finally if it was successfully processed then delete it.

We want to know if the document was posted and if it was correctly processed
or not.

I want to TDD it. So my first thoughts are around doing something like:
```
describe BookingService do
  it 'backs up a document + posts it + keeps track of the status' do
    document = Document.new
    booking_service = BookingService.new(document)
    booking_service.book!

    expect(DocumentBackup.exists?(document.id)).to be_thruty

    expect(FTPClient.exists?(document.name)).to be_thruty

    expect(DocumentBackup.find(document.id).status).to be_pending
  end
end
```

For sure it does not work yet, and now I got two new possible
abstractions: `FTPClient` and `DocumentBackup`. Let's assume they already
exist.

I have some testing questions:
  - Do I want to connect to an ftp folder to check if the file is there?
  - Do I want to ask the storage if it contains my file?

I know that I want to test my plumbing and ensure the data is flowing in
the right parts. So I start with a test that does not mock or stub anything.

But then I
