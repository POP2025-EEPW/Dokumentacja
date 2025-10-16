```
interface IAcquisitionAppGenerator {
  addAcquisitionAppTemplate(template: AcquisitionAppTemplate): string
  editAcquisitionAppTemplate(templateId: string, template: AcquisitionAppTemplate): void
  listAcquisitionAppTemplates(ownerId?: string, includeVersions?: boolean): AcquisitionAppTemplateSummary[]
  generateAcquisitionAppCode(templateId: string, datasetIds: string[], options?: object): AppCodeArtifact
}
```

```
interface IAcquisitionAppManager {
  createAcquisitionApp(appTemplateId: string, datasetIds: string[], params: object): AcquisitionApp
  updateAcquisitionApp(appId: string, params: object): void
  releaseAcquisitionApp(appId: string): void
  startIngestion(appId: string): string                           // jobId
  stopIngestion(appId: string): void
  getAcquisitionApp(appId: string): AcquisitionApp
  listAcquisitionApps(datasetId?: string, ownerId?: string): AcquisitionAppSummary[]
}
```

```
interface IAccessAppGenerator {
  addAccessAppTemplate(template: AccessAppTemplate): string
  editAccessAppTemplate(templateId: string, template: AccessAppTemplate): void
  listAccessAppTemplates(ownerId?: string, includeVersions?: boolean): AccessAppTemplateSummary[]
  generateAccessAppCode(templateId: string, datasetId: string, options?: object): AppCodeArtifact
}
```

```
interface IAccessAppManager {
  deployAccessApp(appCode: AppCodeArtifact, target: DeploymentTarget, params?: object): AccessApp
  updateAccessApp(appId: string, appCode?: AppCodeArtifact, params?: object): void
  enableAccessApp(appId: string): void
  disableAccessApp(appId: string): void
  getAccessApp(appId: string): AccessApp
  listAccessApps(datasetId?: string, ownerId?: string): AccessAppSummary[]
}
```

```
interface ISchemaTransformationEngine {
  addDataSchema(schema: DataSchema): string
  editDataSchema(schemaId: string, schema: DataSchema): void
  listDataSchemas(ownerId?: string, seriesId?: string, includeVersions?: boolean): DataSchemaSummary[]
  addSchemaTransformation(transformation: SchemaTransformation): string
  editSchemaTransformation(transformationId: string, transformation: SchemaTransformation): void
  listSchemaTransformations(ownerId?: string): SchemaTransformationSummary[]
  previewTransformation(transformationId: string, inputSample: DataSample): DataSample
  executeDatasetTransformation(sourceDatasetId: string, transformationId: string, targetSchemaId: string): Dataset
}
```

```
interface IDatasetManager {
  // catalogs & browsing
  addCatalog(parentCatalogId: string | null, catalog: Catalog): string
  getCatalog(catalogId: string): Catalog
  listCatalogs(parentCatalogId?: string | null): CatalogSummary[]
  listCatalogDatasets(catalogId: string, page?: number, pageSize?: number): DatasetSummary[]

  // dataset lifecycle & schema
  addDataset(dataset: Dataset): string
  editDataset(datasetId: string, dataset: Dataset): void
  setDataSchema(datasetId: string, schemaId: string): void
  submitDataset(datasetId: string): void
  publishDataset(datasetId: string): void
  archiveDataset(datasetId: string): void

  // retrieve & search
  getDataset(datasetId: string): Dataset
  listDatasets(filter?: DatasetFilter, page?: number, pageSize?: number): DatasetSummary[]
  getDatasetDescription(datasetId: string): DatasetDescription
  getDatasetPreview(datasetId: string, limit?: number): DatasetPreview
  listOwnedDatasets(ownerId: string): DatasetSummary[]
  listQualityControllableDatasets(controllerId: string): DatasetSummary[]

  // entries & flags
  addDataEntry(datasetId: string, dataEntry: DataEntry): string
  editDataEntry(datasetId: string, entryId: string, dataEntry: DataEntry): void
  markDataEntrySuspicious(datasetId: string, entryId: string, note: string): void
  markDataEntryErroneous(datasetId: string, entryId: string, note: string): void

  //raw datasets
  registerRawDataset(datasetId: string, source: RawSource): string
  getRawDataset(rawDatasetId: string): RawDatasetDescriptor
  appendRawBatch(rawDatasetId: string, batch: RawBatch): string
  listRawBatches(rawDatasetId: string, page?: number, pageSize?: number): RawBatchSummary[]
  getRawDownloadLink(rawDatasetId: string, batchId?: string): DownloadToken
  checkRawAvailability(rawDatasetId: string): AvailabilityStatus
}
```

```
interface IDataQualityManager {
  // quality tagging & alerts
  setQualityTag(datasetId: string, qualityTag: QualityTag): void
  listQualityValidityAlerts(datasetId: string): QualityValidityAlert[]

  // comments & complaints
  addDatasetComment(datasetId: string, comment: DatasetComment): string
  listDatasetComments(datasetId: string): DatasetComment[]
  submitDataRelatedRequest(request: DataRelatedRequest): string
  listDataRelatedRequests(filter?: DataRelatedRequestFilter): DataRelatedRequest[]
}
```

```
interface IDatasetDistributor {
  registerRawDataset(datasetId: string, source: RawSource): string                 // rawDatasetId
  getRawDataset(rawDatasetId: string): RawDatasetDescriptor


  getRawDownloadLink(rawDatasetId: string, batchId?: string): DownloadToken
  checkRawAvailability(rawDatasetId: string): AvailabilityStatus
}
```

```
interface IAuthorizationManager {
  authorize(userId: string, action: string, resourceId: string): AuthorizationDecision
  getUserRoles(userId: string): string[]
}
```