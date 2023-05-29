import express, { Request, Response } from 'express';
import { v4 as uuidv4 } from 'uuid';
import { DateTime } from 'luxon';

interface Contact {
  id: number;
  phoneNumber?: string;
  email?: string;
  linkedId: number | null;
  linkPrecedence: 'primary' | 'secondary';
  createdAt: string;
  updatedAt: string;
  deletedAt?: string | null;
}

interface ConsolidatedContact {
  contact: {
    primaryContactId: number;
    emails: string[];
    phoneNumbers: string[];
    secondaryContactIds: number[];
  };
}

const app = express();
app.use(express.json());

let contacts: Contact[] = [];

app.post('/identify', (req: Request, res: Response<ConsolidatedContact>) => {
  const { email, phoneNumber } = req.body;

  // Check if there are any contacts with matching email or phoneNumber
  const matchingContacts = contacts.filter(
    (contact) => contact.email === email || contact.phoneNumber === phoneNumber
  );

  // If no matching contacts, create a new primary contact
  if (matchingContacts.length === 0) {
    const newContact: Contact = {
      id: contacts.length + 1,
      phoneNumber,
      email,
      linkedId: null,
      linkPrecedence: 'primary',
      createdAt: DateTime.now().toString(),
      updatedAt: DateTime.now().toString()
    };
    contacts.push(newContact);
    res.status(200).json({
      contact: {
        primaryContactId: newContact.id,
        emails: [newContact.email],
        phoneNumbers: [newContact.phoneNumber],
        secondaryContactIds: []
      }
    });
    return;
  }

  // Find the primary contact and collect secondary contact ids
  const primaryContact = matchingContacts.find(
    (contact) => contact.linkPrecedence === 'primary'
  );
  const secondaryContactIds = matchingContacts
    .filter((contact) => contact.linkPrecedence === 'secondary')
    .map((contact) => contact.id);

  // If the request contains new information, create a new secondary contact
  if (
    (email && primaryContact.email !== email) ||
    (phoneNumber && primaryContact.phoneNumber !== phoneNumber)
  ) {
    const newContact: Contact = {
      id: contacts.length + 1,
      phoneNumber,
      email,
      linkedId: primaryContact.id,
      linkPrecedence: 'secondary',
      createdAt: DateTime.now().toString(),
      updatedAt: DateTime.now().toString()
    };
    contacts.push(newContact);
    secondaryContactIds.push(newContact.id);
  }

  res.status(200).json({
    contact: {
      primaryContactId: primaryContact.id,
      emails: [primaryContact.email, ...(email ? [email] : [])],
      phoneNumbers: [primaryContact.phoneNumber, ...(phoneNumber ? [phoneNumber] : [])],
      secondaryContactIds
    }
  });
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
