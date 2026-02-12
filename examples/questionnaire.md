# Questionnaire Examples

## Basic Questionnaire

Generate a simple shareable questionnaire:

```bash
/cgenius questionnaire create "Acme Corporation"
```

**Output:**
```
## Shareable Questionnaire Created ✅

Client: Acme Corporation
Expires: March 14, 2026

Share Link: https://beta.cgenius.app/questionnaire/qst_abc123...
```

## With Email Pre-fill

Pre-fill client email for convenience:

```bash
/cgenius questionnaire create "TechStart Inc" --email "founder@techstart.io"
```

## With Auto-Proposal

Automatically generate proposal when submitted:

```bash
/cgenius questionnaire create "FastGrow Co" \
  --email "ceo@fastgrow.co" \
  --notify "team@agency.com" \
  --auto-proposal
```

## With Custom Branding

Add your agency branding:

```bash
/cgenius questionnaire create "Enterprise Corp" \
  --brand-name "Digital Growth Agency" \
  --brand-logo "https://agency.com/logo.png" \
  --brand-color "#6366F1" \
  --expires 14
```

## Check Status

See if a questionnaire has been submitted:

```bash
/cgenius questionnaire status qst_abc123...
```

## List All Questionnaires

View all your questionnaires:

```bash
# All questionnaires
/cgenius questionnaire list

# Only pending
/cgenius questionnaire list --status pending

# Only completed
/cgenius questionnaire list --status completed
```

## Complete Workflow

1. **Create questionnaire:**
```bash
/cgenius questionnaire create "New Client" \
  --email "contact@newclient.com" \
  --notify "sales@agency.com" \
  --auto-proposal
```

2. **Share link with client** (via email, SMS, or QR code)

3. **Client fills out form** (no login required)

4. **Receive notification** when submitted

5. **Proposal auto-generates** (if enabled)

6. **Review and send** to client
